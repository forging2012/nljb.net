---
title: OpenCV之从video设备录制视频
date: '2017-09-04'
description:
categories:

tags:opencv

---

>

### OpenCV之从video设备录制视频

>


	#include <opencv2/opencv.hpp>
	#include <opencv2/core/core.hpp>
	#include <opencv2/highgui/highgui.hpp>
	#include <QLatin1String>
	#include <QQueue>
	#include <QDateTime>

	using namespace cv;
	using namespace std;

	int main(int argc, char *argv[])
	{

	    int id = QString(QLatin1String(argv[1])).toInt();
	    int fps = QString(QLatin1String(argv[2])).toInt();
	    int len = QString(QLatin1String(argv[3])).toInt();
	    QString nm = QString(QLatin1String(argv[4]));
	    CvCapture* capture = cvCreateCameraCapture(id);
	    cvSetCaptureProperty(capture, CV_CAP_PROP_FRAME_WIDTH, 1280);
	    cvSetCaptureProperty(capture, CV_CAP_PROP_FRAME_HEIGHT, 1024);
	    cvSetCaptureProperty(capture, CV_CAP_PROP_FPS, fps);

	    IplImage* frame = cvQueryFrame(capture);
	    CvVideoWriter* video = cvCreateVideoWriter(nm.toStdString().c_str(), CV_FOURCC('D', 'I', 'V', 'X') , fps, cvSize(frame->width, frame->height));

	    int count = 0;
	    QQueue<qint64> t;
	    qint64 t0 = QDateTime::currentMSecsSinceEpoch();
	    while (count <= len)
	    {
		frame = cvQueryFrame(capture);
		if(!frame)
		{
		    cout<<"Can not get frame from the capture."<<endl;
		    break;
		}
		cvWriteFrame(video, frame);
		count++;
		const qint64 now = QDateTime::currentMSecsSinceEpoch();
		const qint64 dt = now - t0;
		t.enqueue(now);
		if (count % 10 == 0) {
		    cout << QString().sprintf("COUNT %d - FPS(%.1f/%.1f)", count,  (count  *1000.0 / dt), (t.size() * 1000.0 / (now - t.first()))).toStdString().c_str() << endl;;
		}
		if (t.size() > 10)
		    t.dequeue();
	    }
	    cvReleaseVideoWriter(&video);
	    cvReleaseCapture(&capture);
	}

>

