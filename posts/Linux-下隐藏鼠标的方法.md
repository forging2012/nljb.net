---
title: Linux-下隐藏鼠标的方法
date: '2014-07-04'
description:
categories:

tags:system

---

最近工作需要，调查了一下如何在界面启动以后不显示鼠标光标，但是触摸屏可以正常工作，现将方法总结一下:

1,就是使用unclutter,这个可以起作用，但是不是我想要的效果；

	./unclutter: usage:
	    -display <display>
	    -idle <seconds>        time between polls to detect idleness.
	    -keystroke        wait for keystroke before idling.
	    -jitter <pixels>    pixels mouse can twitch without moving
	    -grab            use grabpointer method not createwindow
	    -reset            reset the timer whenever cursor becomes
			    visible even if it hasn't moved
	     -root                   apply to cursor on root window too
	    -onescreen        apply only to given screen of display
	     -visible               ignore visibility events
	     -noevents              don't send pseudo events
	    -regex            name or class below is a regular expression
	    -not names...        don't apply to windows whose wm-name begins.
			(must be last argument)
	    -notname names...    same as -not names...
	    -notclass classes...    don't apply to windows whose wm-class begins.
			(must be last argument, cannot be used with
			-not or -notname)

	原因事它总有一刹那是出现鼠标的；

2：xsetroot -cursor blank.bmp blank.bmp 

	其中blank.bmp是使用bitmap &制作的的一个空白鼠标；
	效果是可以在桌面上起作用，但是在程序界面鼠标依然出现；

3：setterm -cursor off关闭终端界面的光标；

4：oneko一个类似于unclutter的程序；

5：在调用startx启动的桌面系统中给XSERVER传递参数serverargs=" -nocursor "，这个可以起作用；

	在211行执行xinit的地方, 在 "$server"后面加上 -nocursor
	xinit "$client" $clientargs -- "$server" -nocursor  $display $serverargs


