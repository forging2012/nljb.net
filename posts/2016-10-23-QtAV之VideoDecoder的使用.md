---
title: QtAV之VideoDecoder的使用
date: '2016-10-23'
description:
categories:

tags:qt

---

>

### VideoDecoder是QtAV的解码器

>

***本文主要目的是介绍，如何独立使用 VideoDecoder 解码 ... 自定义绘制输出 ..***

>

	VideoDecoder 支持 "FFmpeg", "CUDA", "VDA", "VAAPI", "DXVA", "Cedarv" 解码方式

>

---

>

### VideoDecoder解码（官方案例）

>

***请关注文中注释部分***

>

    #include <QCoreApplication>
    #include <QtDebug>
    #include <QtCore/QDateTime>
    #include <QtCore/QQueue>
    #include <QtCore/QStringList>
    #include <QtAV/AVDemuxer.h>
    #include <QtAV/VideoDecoder.h>
    #include <QtAV/Packet.h>

    using namespace QtAV;

    int main(int argc, char *argv[])
    {
		// ---------------------------------
		// QCoreApplication 只能出现在主线程中 ...
		// ---------------------------------
        QCoreApplication a(argc, argv);
        QString file = QString::fromLatin1("test.avi");
        int idx = a.arguments().indexOf(QLatin1String("-f"));
        if (idx > 0)
            file = a.arguments().at(idx + 1);
		// ---------------------------------
		// 使用那种解码器 FFmpeg 默认软解
		// "FFmpeg", "CUDA", "VDA", "VAAPI", "DXVA", "Cedarv"
		// ---------------------------------
        QString decName = QString::fromLatin1("FFmpeg");
        idx = a.arguments().indexOf(QLatin1String("-vc"));
        if (idx < 0)
            idx = a.arguments().indexOf(QLatin1String("-vd"));
        if (idx > 0)
            decName = a.arguments().at(idx + 1);

        QString opt;
        QVariantHash decopt;
        idx = decName.indexOf(QLatin1String(":"));
        if (idx > 0) {
            opt = decName.right(decName.size() - idx -1);
            decName = decName.left(idx);
            QStringList opts(opt.split(QString::fromLatin1(";")));
            QVariantHash subopt;
            foreach (QString o, opts) {
                idx = o.indexOf(QLatin1String(":"));
                subopt[o.left(idx)] = o.right(o.size() - idx - 1);
            }
            decopt[decName] = subopt;
        }
        qDebug() << decopt;

        VideoDecoder *dec = VideoDecoder::create(decName.toLatin1().constData());
        if (!dec) {
            fprintf(stderr, "Can not find decoder: %s\n", decName.toUtf8().constData());
            return 1;
        }
		// -------------------------------------------------------------
		// 自动释放 ...
		QScopedPointer<VideoDecoder> decoder;
		decoder.reset(dec);
		// -------------------------------------------------------------
        if (!decopt.isEmpty())
            dec->setOptions(decopt);
        AVDemuxer demux;
        demux.setMedia(file);
        if (!demux.load()) {
            qWarning("Failed to load file: %s", file.toUtf8().constData());
            return 1;
        }
        dec->setCodecContext(demux.videoCodecContext());
		// -------------------------------------------------------------
		// 请注意这里是官方对 open 与 close 的要求，不是线程安全的，最好使用锁 ...
		/*!
		* default is open FFmpeg codec context
		* codec config must be done before open
		* NOTE: open() and close() are not thread safe. You'd better call them in the same thread.
		*/
		// ---------------------------------------------------------------
        dec->open();
        int count = 0;
        int vstream = demux.videoStream();
        QQueue<qint64> t;
        qint64 t0 = QDateTime::currentMSecsSinceEpoch();
        while (!demux.atEnd()) {
            if (!demux.readFrame())
                continue;
            if (demux.stream() != vstream)
                continue;
            const Packet pkt = demux.packet();
            if (dec->decode(pkt)) {
                VideoFrame frame = dec->frame(); // why is faster to call frame() for hwdec? no frame() is very slow for VDA
				// ---------------------------------
				// 使用前最好判断一下，帧的正确性 ...
				if (!frame.isValid()) {
                    continue;
                }
                // 注意：此处将帧保存 ... 备用 ... 缓存帧数不要太多 ... 1080P(10+-) 4k(5-) 等 ... 以免显存不足
				// 比如: frames 需自定义，frames 读写需加锁 ...
				for (;;) {
					 if (frames.size < 5) {
                        break;
                    }
                    QThread::msleep(10);
				}
				frames.push(frame);
				// ---------------------------------
                Q_UNUSED(frame);
                count++;
                const qint64 now = QDateTime::currentMSecsSinceEpoch();
                const qint64 dt = now - t0;
                t.enqueue(now);
                printf("decode count: %d, elapsed: %lld, fps: %.1f/%.1f\r", count, dt, count*1000.0/dt, t.size()*1000.0/(now - t.first()));fflush(0);
                if (t.size() > 10)
                    t.dequeue();
            }
        }
		// ---------------------------------
		// 解码器释放部分 ...
		dec->flush();
		demux.setInterruptStatus(-1);
		demux.unload();
		// setCodecContext 相当于 close 需要加锁
		decoder->setCodecContext(0);
		decoder.reset(0);
		// ---------------------------------
        return 0;
    }

>

---

>

### 如何使用 VideoFrame

>

***三种使用VideoFrame思路***

* OpenGL 渲染 VideoFrame 帧
* SDL 绘制 VideoFrame 帧（需转换格式）
* QImage 绘制 VideoFrame 帧（需转换格式）(效率低)

>

#### QImage

	// 就不多介绍了 ... 
	QImage image = frame.toImage();
	QImage image = frame.toImage(QImage::Format_ARGB32);

>

### SDL

	// 事先需要将帧转为SDL需要输出的格式，例如：Format_YUV420P
	frame = frame.to(VideoFormat::PixelFormat::Format_YUV420P);

	// 将 VideoFrame 转为 AVPicture ...
	AVPicture* Decoder::convert(VideoFrame &inFrame, int w, int h)
	{
		AVPicture *pFrameYUV = new AVPicture();
		AVPicture pFrame;
		avpicture_fill(&pFrame, (uint8_t*) inFrame.bits(), AV_PIX_FMT_YUV420P, w, h);
		avpicture_alloc(pFrameYUV, AV_PIX_FMT_YUV420P, w, h);
		av_picture_copy(pFrameYUV, &pFrame, AV_PIX_FMT_YUV420P, w, h);
		return pFrameYUV;
	}

	// 如何用SDL绘制一个AV_PIX_FMT_YUV420P的AVPicture就不用我多说了吧 ....
	SDL_UpdateTexture(texture, NULL, avp->data[0], avp->linesize[0]);
	SDL_RenderCopy(renderer, texture, NULL, rect);
	SDL_RenderPresent(renderer);

	// 注意：AVPicture 需要手动释放，但不能在 SDL_RenderPresent 前释放 ...
	// 建议：最后有个释放队列，绘制完的帧都从绘制队列移到释放队列 ...

### OpenGL

	// 不用多说, 看代码 ...

	// 使用 QtAV 的 OpenGLVideo 渲染 ...
	OpenGLVideo oglv;
	oglv.setOpenGLContext(QOpenGLContext::currentContext());
	oglv.setCurrentFrame(frame);
	oglv.setProjectionMatrixToRect(rectf);
	oglv.render();

	// 如果想使用自定义的OGL渲染VideoFrame那研究研究OpenGLVideo吧 ...

 >

>

### 结语

>

	QtAV 如何如何好，我就不夸了 ... 但只能说是假开源 ... 代码一坨看得懂叫开源看不懂跟看不到一样 ..

>