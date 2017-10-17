---
title: OpenCV之基于混合高斯模型GMM的运动目标检测
date: '2017-10-17'
description:
categories:

tags:opencv

---

>

### 基于混合高斯模型GMM的运动目标检测

>

	#include <iostream>  
	#include <string>  
	  
	#include <opencv2/opencv.hpp>  
	  
	  
	int main(int argc, char** argv)  
	{  
	    std::string videoFile = "../test.avi";  
	  
	    cv::VideoCapture capture;  
	    capture.open(videoFile);  
	  
	    if (!capture.isOpened())  
	    {  
		std::cout<<"read video failure"<<std::endl;  
		return -1;  
	    }  
	  
	  
	    cv::BackgroundSubtractorMOG2 mog;  
	  
	    cv::Mat foreground;  
	    cv::Mat background;  
	  
	    cv::Mat frame;  
	    long frameNo = 0;  
	    while (capture.read(frame))  
	    {  
		++frameNo;  
	  
		std::cout<<frameNo<<std::endl;  
	  
		// 运动前景检测，并更新背景  
		mog(frame, foreground, 0.001);         
		  
		// 腐蚀  
		cv::erode(foreground, foreground, cv::Mat());  
		  
		// 膨胀  
		cv::dilate(foreground, foreground, cv::Mat());  
	  
		mog.getBackgroundImage(background);   // 返回当前背景图像  
	  
		cv::imshow("video", foreground);  
		cv::imshow("background", background);  
	  
	  
		if (cv::waitKey(25) > 0)  
		{  
		    break;  
		}  
	    }  
	      
	    return 0;  
	}  

>

    // OpenCV2
    // --------------------------------------------------
    BackgroundSubtractorMOG2 m_mog;
    SimpleBlobDetector::Params m_params;
    m_params.blobColor = 255;
    SimpleBlobDetector m_detector(m_params);
    Ptr<SimpleBlobDetector> m_blob = m_detector.create("SimpleBlob");
    // --------------------------------------------------
    int count = 0;
    // -----------------------
    Rect rect = cv::Rect(150, 50, 350, 250);
    Mat frame;
    Mat foreground;
    while(m_cap.read(frame))
    {
        // ------------------------------
        // 运动前景检测
        // m_mog->apply(frame(rect), foreground, 0.001);
        m_mog(frame(rect), foreground, 0.001);
        // 腐蚀
        cv::erode(foreground, foreground, cv::Mat());
        // 膨胀
        cv::dilate(foreground, foreground, cv::Mat());
        // getBackgroundImage
        // m_mog->getBackgroundImage(background);
        // -------------------------------------------------------------- //
        // 特征点检测
        vector<KeyPoint> key_points;
        m_blob->detect(foreground, key_points);
        // -------------------------------------------------------------- //
        // Debug
        if (Runtime::m_configure->m_parcel_debug)
        {
            Mat channel = Mat::zeros(foreground.rows, foreground.cols, CV_8UC3);
            vector<cv::Mat> channels;
            channels.push_back(foreground);
            channels.push_back(foreground);
            channels.push_back(foreground);
            cv::merge(channels, channel);
            cv::resize(channel, channel,Size(frame.cols, frame.rows), 0, 0, cv::INTER_LINEAR);
            addWeighted(frame, 0.5, channel, 0.5, 0, frame);
            m_frames->push(Frame(frame.clone(), count));
        }
        // -------------------------------------------------------------- //
    }

>


	// OpenCV3
	#include <opencv2/features2d/features2d.hpp>
	#include <opencv2/core.hpp>
	#include <opencv2/core/utility.hpp>
	#include <opencv2/imgproc.hpp>
	#include <opencv2/video/background_segm.hpp>
	#include <opencv2/videoio.hpp>
	#include <opencv2/highgui.hpp>
	#include <iostream>
	#include <QDateTime>

	using namespace std;
	using namespace cv;

	int main(int argc, char** argv)
	{

	    std::string videoFile = "/home/danoo/x.avi";

	    cv::VideoCapture m_cap;
	    m_cap.open(videoFile);
	    if (!m_cap.isOpened())
	    {
		std::cout<<"read video failure"<<std::endl;
		return -1;
	    }

	    m_cap.set(CV_CAP_PROP_FRAME_WIDTH, 640);
	    m_cap.set(CV_CAP_PROP_FRAME_HEIGHT, 480);
	    // m_cap.set(CV_CAP_PROP_FPS, Runtime::m_configure->m_render_fps);
	    // m_cap.set(CV_CAP_PROP_FOURCC, CV_FOURCC('M','G','P','G'));
	    // ------------------------------------ //
	    Ptr<cv::BackgroundSubtractor> m_mog;
	    m_mog = createBackgroundSubtractorMOG2();
	    // ------------------------------------ //
	    SimpleBlobDetector::Params m_params;
	    //        params.minThreshold = 20;//二值化的起始阈值
	    //        params.maxThreshold = 180;//二值化的终止阈值
	    //        params.thresholdStep = 10;//二值化的阈值步长
	    //        params.minConvexity = 0.5f;//斑点的最小凸度 默认0.95f
	    //        params.minInertiaRatio = 0.03f;//斑点的最小惯性率 默认0.1f
	    //        params.minArea = 120;//斑点的最小面积
	    //        params.maxArea = 5000;//斑点的最大面积
	    m_params.blobColor = 255;//检测白色
	    //   重复的最小次数，只有属于灰度图像斑点的那些二值图像斑点数量大于该值时，该灰度图像斑点才被认为是特征点
	    //   params.minRepeatability = 2;
	    //   最小的斑点距离，不同二值图像的斑点间距离小于该值时，被认为是同一个位置的斑点，否则是不同位置上的斑点
	    //   params.minDistBetweenBlobs = 10;
	    Ptr<SimpleBlobDetector> m_detector;
	    m_detector = SimpleBlobDetector::create(m_params);
	    // -----------------------------------------------------------------------------

	    namedWindow("video", WINDOW_NORMAL);
	    //    namedWindow("background", WINDOW_NORMAL);
	    namedWindow("SimpleBlobDetector");

	    int m_before_size;
	    Mat frame;
	    Mat foreground;
	    //    Mat background;
	    while(m_cap.read(frame))
	    {
		// ------------------------------ //
		// 运动前景检测
		m_mog->apply(frame(cv::Rect(150, 50, 350, 250)), foreground, 0.001);
		// 腐蚀
		cv::erode(foreground, foreground, cv::Mat());
		// 膨胀
		cv::dilate(foreground, foreground, cv::Mat());
		// getBackgroundImage
		//        m_mog->getBackgroundImage(background);
		// 特征点检测
		vector<KeyPoint> key_points;
		m_detector->detect(foreground, key_points);
		if (m_before_size != -1 && m_before_size >= 3 && m_before_size > key_points.size()) {
		    // m_log->debug(QString().sprintf("Parcel Point Size -> %d", m_before_size));
		    cv::rectangle(frame, cv::Rect(150, 50, 350, 250), CV_RGB(255, 0, 0), 4, 8, 0);
		    // --------------------------------------------------------------------------------
		    // m_frames->push(Frame(frame.clone(), count));
		}
		printf("%d(%d)\n", key_points.size(), m_before_size);
		m_before_size = key_points.size();
		// -------------------------------
		cv::imshow("video", frame);
		//        cv::imshow("background", background);
		cv::imshow("SimpleBlobDetector", foreground);
		if (cv::waitKey(25) > 0)
		    break;
	    }

	    return 0;
	}

>
