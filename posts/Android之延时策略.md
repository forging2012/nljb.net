---
title: Android之延时策略
date: '2015-12-07'
description:
categories:

tags:android

---

>

### 延时策略

>

	所谓延时策略也就是让一个动作Sleep一下再执行

	有这么几种方法

>

---

>

	// 线程休眠
	new Thread(new Runnable(){   
		public void run(){   
			Thread.sleep(XXXX);   
			handler.sendMessage(); //告诉主线程执行任务   
		}   
	}).start 

>

---

>

	// 定时器
	TimerTask task = new TimerTask(){   
		public void run(){   
			// execute the task 
		}   

	};   
	Timer timer = new Timer(); 
	timer.schedule(task, delay); 

>

---

>

	// Handler 延时消息机制
	new Handler().postDelayed(new Runnable(){   
		public void run() {   
			// execute the task   
		}   
	 }, delay);   

>

	...
