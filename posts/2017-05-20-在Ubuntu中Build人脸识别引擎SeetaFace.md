---
title: 在Ubuntu中Build人脸识别引擎SeetaFace
date: '2017-05-20'
description:
categories:

tags:system

---

>

#### 在Ubuntu中Build人脸识别引擎SeetaFace

>

***SeetaFace是中科院计算机所山世光老师所带领的团队开发出来的人脸识别库，开源免费可用，据说识别率可达97.1%，实测下来识别率确实是很高***

>

    FaceDetection
        人脸识别模块，用于识别出照片中的人脸，染回每个人脸的坐标和人脸总数
    FaceAlignment
        特征点识别模块，主要识别两个嘴角、鼻子、两个眼睛五个点的坐标
    FaceIdentification
        人脸比较模块，根据官方的说法，先提取特征值，然后比较。

>

    // 下载
    // 解压获得 SeetaFaceEngine-master/FaceDetection
    // 解压获得 SeetaFaceEngine-master/FaceAlignment
    // 解压获得 SeetaFaceEngine-master/FaceIdentification
    https://github.com/nulijiabei/SeetaFaceEngine（Download SeetaFace）
    
>

**编译顺序：FaceDetection、FaceAlignment、FaceIdentification**

>

    说明：只要编译成功，运行时出现段错误等 ... 都是相关文件路径配置错误

>

    sudo apt-get install build-essential
    sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
    sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

>

    // 下载 OpenCV 不要使用 v3.x(测试失败) 使用 2.4.13.2(经过测试) ...
    http://opencv.org/releases.html
    
>
    
    // 编译 OpenCV
    cd ~/opencv
    mkdir release
    cd release
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..

>

    // 编译 FaceDetection
    cd SeetaFaceEngine-master/FaceDetection
    mkdir build
    cd build  
    cmake ..  
    make -j${npoc}
    
    // 使用 FaceDetection
    ./build/facedet_test imagefilePath ./model/seeta_fd_frontal_V1.0.bin
    
    // 注意
    error: ‘isnan’ was not declared in this scope 
    解决方法，修改文件中的isnan为“std::isnan”
    
>

    // 编译 FaceAlignment
    cd SeetaFaceEngine-master/FaceAlignment
    mkdir build
    cd build  
    cmake .. 
    make
    
    // 准备工作
    拷贝FaceDetection中的/include/face_detection.h到FaceAlignment的include
    拷贝FaceDetection中的/build/libseeta_facedet_lib.so文件到FaceAlignment/build
    拷贝FaceDetection/model文件夹下的seeta_fd_frontal_v1.0.bin文件到FaceAlignment的build
    拷贝FaceAlignment中的model文件夹和data文件夹到FaceAlignment/build
    
>

    // 编译 FaceIdentification
    cd SeetaFaceEngine-master/FaceIdentification
    mkdir build
    cd build
    cmake .. && make
    
    // 准备工作
    拷贝FaceDetection中的/include/face_detection.h到FaceIdentification的include
    拷贝FaceDetection中的/build/libseeta_facedet_lib.so文件到FaceIdentification/build
    拷贝FaceAlignment中的/include/face_alignment.h到FaceIdentification的include
    拷贝FaceAlignment中的/build/libseeta_fa_lib.so文件到FaceIdentification/build

    // 准备工作 +
    修改FaceIdentification下src/test/CMakeLists.txt
    ......
    add_executable(${BIN} ${f}) // 原有参照
    target_link_libraries(${BIN} viplnet ${OpenCV_LIBS}) // 原有参照
    target_link_libraries(${BIN} viplnet ${OpenCV_LIBS} /实际路径/SeetaFaceEngine-master/FaceIdentification/linux/libseeta_facedet_lib.so)
    target_link_libraries(${BIN} viplnet ${OpenCV_LIBS} /实际路径/SeetaFaceEngine-master/FaceIdentification/linux/libseeta_fa_lib.so)
    ......
    
    // 准备工作 +
    拷贝FaceDetection/model文件夹下的seeta_fd_frontal_v1.0.bin文件到FaceIdentification的build
    拷贝FaceAlignment/model文件夹下的seeta_fa_v1.1.bin文件到文件到FaceIdentification的build/model
    
>

    // 项目中Linux系统路径配置都是./而不是../所以最好将./data/和./model拷贝到build目录中
    // 当然你也可以修改这里的配置 ...
    #ifdef _WIN32
    std::string DATA_DIR = "../../data/";
    std::string MODEL_DIR = "../../model/";
    #else
    std::string DATA_DIR = "./data/";
    std::string MODEL_DIR = "./model/";
    #endif
    
    // FaceIdentification 中 model/seeta_fr_v1.0.bin 是需要解压缩的哦
    apt-get install rar // 然后解压 .. 到当前目录

>

    // 最后重申：只要编译过了 ... 运行段错误，一定是.bin文件路径配置错误造成的 ...

>

---

>

    // OpenCV 默认只支持 avi 格式视频读取 ... 如果需要兼容 mp4、mov、等格式 ... 需要 ffmepg

    sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next 
    sudo apt-get update 
    sudo apt-get install ffmpeg

    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_TIFF=ON -D CMAKE_CXX_FLAGS=-D__STDC_CONSTANT_MACROS ..

>

