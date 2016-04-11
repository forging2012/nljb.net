---
title: CxImage
date: '2014-09-30'
description:
categories:

tags:c++

---

CxImage类库是一个优秀的图像操作类库。

可以快捷地存取、显示、转换各种图像。

---

添加头文件目录

	CxImage\Include

加库文件目录

	CxImage\lib

添加链接

	cximage.lib
	demod.lib
	j2k.lib
	jasper.lib
	jbig.lib
	jpeg.lib
	png.lib
	tiff.lib
	zlib.lib

序中添加头文件

	#include "ximage.h"

---

基本用法

	// 打开
	CxImage image;
	if(image.load("name",类型))
	{
	       CDC *pDC = GetDC();
	       image.Draw(pDC->GetSafeHDC(),CRect rect(0,0,100,100));
	       pDC->DeleteDC;     
	}
	　　

	// 旋转
	CxImage smallImage; // 旋转后的图片
	image.Rotate(90,&smallImage); // 旋转90，并且保存到smallImage中
	smallImage.Save(保存的名字,类型);

	// 镜像
	CxImage ImgTmp = image;
	if(ImgTmp.Mirror())
	{
	     image.Draw(....  ,  .....);
	}

	// 缩放
	CxImage samllImg;
	image.Resample(新宽度,新高度,0,&smallImg);
	smallImg.Save("自定第一大小.jpg",CXIMAGE_SUPPORT_JPG);

	// 剪辑
	CDC *pDC=GetDC();
	UpdateData();
	CxImage smallImg;
	tempimage.Crop(CRect(m_xTop,m_yTop,m_xWidth,m_yHeiht),&smallImg);
	smallImg.Save("剪辑图片.jpg",CXIMAGE_SUPPORT_JPG);
	smallImg.Draw(pDC->GetSafeHdc(),CRect(40,70,picwidth,picheight));
	pDC->DeleteDC();

	// 混合
	CDC *pDC=GetDC();
	CxImage smallImg;
	smallImg.Load("混合源文件.jpg",CXIMAGE_SUPPORT_JPG);
	tempimage.Mix(smallImg,CxImage::OpAvg,0,0,true);
	tempimage.Save("Mix混合.jpg",CXIMAGE_SUPPORT_JPG);
	tempimage.Draw(pDC->GetSafeHdc(),CRect(40,70,picwidth,picheight));

---

编译

	// 下载cximage599c_tar包
	http://www.xdp.it/download.htm。

	// 因为是新的版本，编译和网上一些教程有差异。直接使用：
	./configure --prefix=/usr/ --enable-shared （生成并使用静态库）
	// 或者
	./configure --prefix=/usr/ --disable-shared （生成并使用动态库）
	// make
	// make install
	// 大功告成。
