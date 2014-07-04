---
title: SDL-在屏幕上输出一张图片
date: '2014-07-03'
description:
categories:

tags:ffmpeg

---

这篇教程讲述SDL风格的Hello World。
假设你已经安装好了SDL库，下面我们制作一个简陋的图片（640*480像素的图片，并命名为hello.bmp），应用SDL把它加载并显示到屏幕上。
	 
	//文件名：hello.c，以下内容可以全部复制到源文件中。
	//包含SDL函数和数据类型
	#include "SDL/SDL.h"

	/*
	在源文件的开头，包含SDL的头文件，这样，我们就可以使用SDL的函数和数据类型。
	如果有人（比如说使用Visual Studio的）打算这样包含SDL的头文件：#include "SDL.h"。
	如果编译器向你报错说找不到“SDL/SDL.h”的头文件，那么是因为你包含的头文件路径错了，或者说，你没有把SDL.h放到正确的路径下。
        */
	
	int main( int argc, char* args[] )
	{
	    //The images
	    SDL_Surface* hello = NULL;
	    SDL_Surface* screen = NULL;

	/*
	在main()函数的前部，声明两个SDL_Surface的指针。一个SDL_Surface就是一个图片，在这个程序中，我们打算处理两个图片。
	画面指针 “hello”是我们准备加载和显示的图片，“screen”是屏幕上显示出来的范围。
	不论你打算用指针去做什么，始终要记得初始化指针。
	当然，使用SDL的时候，你必须像上面那样声明main函数，不能用void main()或者其他类似的声明。
        */

	    //启动SDL
	    SDL_Init( SDL_INIT_EVERYTHING );
	  
	    //建立屏幕
	    screen = SDL_SetVideoMode( 640, 480, 32, SDL_SWSURFACE );
	  
	    //加载图片
	    hello = SDL_LoadBMP( "hello.bmp" );

	/*
	mian()函数里调用的第一个函数是SDL_Init()，目的是调用SDL_Init()来初始化所有的SDL子系统，这样我们才能开始使用SDL的图形函数。
	下一个函数是SDL_SetVideoMode()，调用它来设置一个640像素宽，480像素高的窗口，每个像素32位。
	最后一个参数(SDL_SWSURFACE)用来在软件存储中建立一个画面。
	当执行完SDL_SetVideoMode()以后，会返回一个我们能够使用的指针，指向窗口画面， 窗口建立以后，我们调用SDL_LoadBMP()来载入图片。
	SDL_LoadBMP()函数接受一个位图文件的路径作为参数，并返回一个指针，该指针指向已经载入的画面。当载入图片发生错误时，该函数返回一个空指针
	*/

	    //把图片放置到屏幕上
	    SDL_BlitSurface( hello, NULL, screen, NULL );
	  
	    //刷新屏幕
	    SDL_Flip( screen );
	  
	    //暂停，等待
	    SDL_Delay( 2000 );

	/*
	现在，我们已经建立了一个窗口，并加载了图片，我们准备把加载的图片放置到屏幕上面。这个是通过SDL_BlitSurface()函数实现的。
	SDL_BlitSurface()的第一个参数是源画面的指针（源画面位置），第三个参数是目的画面的指针（目的画面的位置）。
	SDL_BlitSurface()函数把源画面粘贴到目的画面上。这样，就能够把加载的图片放置到屏幕上显示出来。稍后的教程里会介绍其他参数的作用。
	现在我们的图片已经放置到屏幕上了，我们需要用SDL_Flip()函数来刷新屏幕，这样我们才能看到它。
	如果你没有调用SDL_Flip()函数，那你只能看到一个还没有刷新的黑黑的屏幕。
	到这里，图片已经放置到屏幕上，并显示出来了，我们必须让显示出来的窗口能够停留，不会一闪而过，消失不见。
	调用SDL_Delay()函数，让窗口保持稳定。这里，我们让窗口延时2000毫秒（2秒）。
	在接下来的教程的第四课里，你会学到更好的办法，来让窗口停留在一个地方。
	*/

	    //释放掉内存中加载的图片
	    SDL_FreeSurface( hello );
	  
	    //退出 SDL
	    SDL_Quit();
	  
	    return 0;
	}

	/*
	接下来，我们在程序里将不再使用加载的图片，我们需要把它从内存中释放掉。
	你不能够简单的使用delete释放内存，而是需要调用SDL_FreeSurface()函数把它从内存中释放掉。
	在程序结束时，我们调用SDL_Quit()来关闭SDL程序。你也许或疑惑，为什么我们从来没有释放屏幕画面的内存？别担心，SDL_Quit()会为你释放掉它。
	祝贺你！成功的运行你第一个图形程序。
	*/

	/*
	使用wxDev-C++编译的连接器命令为：-lmingw32 -lSDLmain  -lSDL
	测试编译运行OK！在解释的文字两端加了注视符号，所以这些内容可以全部复制到源程序里。
	*/
	 
	SDL程序编译出错的解决办法

	如果你的编译器向你报错没有找到'SDL/SDL.h'，意味着你忘记了设置头文件。你的编译器或者是IDE应该能都找到SDL的头文件，所以要确认SDL的头文件正确的配置到include文件夹内。如果你使用Visual Studio而编译器报错：'SDL/SDL.h': No such file or directory，查看源文件顶端的代码，确认是使用的#include "SDL.h".

	如果你的程序能够编译，但是连接器会报错找不到库文件，那么确认你的编译器或者IDE能够找到SDL的库文件。如果你的编译器报错没有定义引用一大堆SDL的函数，请确认你在连接器里连接了SDL。

	    如果你的连接器报错是关于指针入口的，那么确认mian函数的声明方式是否正确，在你的源码里，只能有一个main函数。

	    如果程序编译、链接、生成都正常，但是当你运行的时候报错，没有找到SDL.dll，确认一下是否将SDL.dll放置到了和编译执行相同的目录下。使用Visual Studio，需要把dll文件放置到与vcproj文件相同的目录下。如果使用Windows系统，可以把dll文件放到system32目录下。

	如果你的程序可以运行，但是没有显示图片，或者窗口一闪而过，你可以看到stderr.txt文件的内容为：
	Fatal signal: Segmentation Fault (SDL Parachute Deployed)

	这是因为程序试图访问不允许访问的内存。可能是当调用SDL_BlitSurface()时，试图访问NULL。这意味着你需要确认位图文件和程序在同一个目录内。使用Visual Studio需要把位图文件和vcproj文件放在同一个目录内。

	也有可能在你使用Visual Studio时显示这样的错误："The application failed to start because the application configuration is incorrect. Reinstalling the application may fix this problem."（程序无法启动，重新安装来解决此问题）

	这是因为没有安装升级服务包。不要忘记采用最新版本的编译器或者IDE时，安装编译器或IDE的服务升级包，否则Visual Studio编译的程序不会运行。一些Linux的用户运行后可能会显示黑屏，尝试在命令行运行一下程序。

	如果你必须建立一个工程来编译SDL程序，记得你需要给你建立的每一个SDL程序建立一个工程。或者，更进一步，你可以重新使用你第一次建立的工程。


