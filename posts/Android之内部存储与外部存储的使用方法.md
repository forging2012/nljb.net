---
title: Android之内部存储与外部存储的使用方法
date: '2015-03-19'
description:
categories:

tags:android

---

>

### 内部存储

>

Android提供了openFileOutput和openFileInput方法读取设备上的文件

>

        //确定要操作文件的文件名
        String FILE_NAME = "tempfile.tmp";

        // 初始化，如果调用FileOutputStream时指定的文件不存在，
        // Android会自动创建它（在data/包名/files/目录下会创建txtme.txt文件)
        // 在默认情况下，写入的时候会覆盖原文件内容，
        // 如果想把新写入的内容附加到原文件内容后，则应指定其模式为Context.MODE_APPEND

        // MODE_PRIVATE的文件是应用程序私有的 ，
        // MODE_WORLD_READABLE则所有应用程序都可以访问的，
        // MODE_WORLD_WRITEABLE所以应用程序都可以写

        // 创建写入流
        try {
            FileOutputStream fos = openFileOutput("nljb" , Context.MODE_PRIVATE);
            fos.write("nljb".toString().getBytes());
            fos.flush();
            fos.close();
        } catch (java.io.IOException e) {
            e.printStackTrace();
        }

        // 创建读取流
        try {
            FileInputStream fis = openFileInput("nljb");
            byte[] bytes = new byte[fis.available()];
            fis.read(bytes);
            fis.close();
        } catch (java.io.IOException e) {
            e.printStackTrace();
        }

---

>

### 外部存储

>


	// 写入文件
	File dir = Environment.getExternalStorageDirectory();
	File file = new File(dir, "nljb");
	Log.i("INFO", dir.toString());
	try {
	    if (!file.exists())
	    {
		file.createNewFile();
	    }
	    FileOutputStream fos = new FileOutputStream(file);
	    fos.write("nljb".toString().getBytes());
	    fos.flush();
	    fos.close();
	} catch (java.io.IOException e) {
	    e.printStackTrace();
	}

	// 读取文件
	File dir = Environment.getExternalStorageDirectory();
	File file = new File(dir, "nljb");
	try {
	    if (!file.exists())
	    {
		file.createNewFile();
	    }
	    FileInputStream fis = openFileInput(file);
	    byte[] bytes = new byte[fis.available()];
	    fis.read(bytes);
	    fis.close();
	} catch (java.io.IOException e) {
	    e.printStackTrace();
	}


---

>

# Assets 目录介绍

>

Assets 这个目录保存的文件可以打包在程序里

/res和/assets的不同点是，android不为/assets下的文件生成ID。

如果使用/assets下的文件，需要指定文件的路径和文件名。

>

android中的资源文件，这些资源文件主要分为两类，一种出于asset目录下，称为原生文件

这类文件在被打包成apk文件时是不会进行压缩的；

另一类则是res下的文件，这类文件在打包成apk文件时，会进行小内存优化

	AssetManager assetManager = this.getResources().getAsset();

在activity中可以通过如下方法进行访问

	InputStream inputStream = Resources.openRawResource(int id);

asset和res下的文件都是只能读不能写

