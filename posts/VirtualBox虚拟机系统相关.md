---
title: VirtualBox虚拟机系统相关
date: '2015-12-02'
description:
categories:

tags:system

---

>

### VirtualBox虚拟机系统相关

>

---

>

### 在Win7系统中自动启动虚拟机系统

>

	// 写个BAT批处理, 开机运行, 并且自动进入全屏模式
	@ECHO OFF
	start D:\dev\VirtualBox\VirtualBox.exe --startvm ubuntu12.04 --fullscreen
	EXIT

>

---

>

### 通过PowerShell 启动 VirtualBox 与强制另外一个程序最前端显示

>

	Add-Type @"
	[DllImport("user32.dll")]  
	public static extern IntPtr FindWindow(string lpClassName, string lpWindowName);  
	public static IntPtr FindWindow(string windowName){
		return FindWindow(null,windowName);
	}
	[DllImport("user32.dll")]
	public static extern bool SetWindowPos(IntPtr hWnd, 
	IntPtr hWndInsertAfter, int X,int Y, int cx, int cy, uint uFlags);
	[DllImport("user32.dll")]  
	public static extern bool ShowWindow(IntPtr hWnd, int nCmdShow); 
	static readonly IntPtr HWND_TOPMOST = new IntPtr(-1);
	static readonly IntPtr HWND_NOTOPMOST = new IntPtr(-2);
	const UInt32 SWP_NOSIZE = 0x0001;
	const UInt32 SWP_NOMOVE = 0x0002;
	const UInt32 TOPMOST_FLAGS = SWP_NOMOVE | SWP_NOSIZE;
	[DllImport("user32.dll", CharSet = CharSet.Auto)]
	public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
	[DllImport("user32.dll", CharSet = CharSet.Auto)]
	public static extern bool SetForegroundWindow(IntPtr hWnd);
	[DllImport("user32.dll", CharSet = CharSet.Auto)]
	public static extern int MoveWindow(IntPtr hWnd, int x, int y, int nWidth, int nHeight, bool BRePaint);
	public static void RunMoveWindow(IntPtr hWnd, int x, int y, int nWidth, int nHeight) {
		SetForegroundWindow(hWnd);
		SetWindowPos(hWnd, HWND_TOPMOST, 0, 0, 0, 0, TOPMOST_FLAGS);
		MoveWindow(hWnd, x, y, nWidth, nHeight, true);
	}
	"@  -name “Auto” -namespace Win32API

	# 设置窗口大小及位置
	Function Set-Start
	{
		# 初始化参数
		param(
			[Parameter(Mandatory=$true, ValueFromPipeline=$true)]
			# 获取目标窗体的句柄
			[System.Diagnostics.Process]$Process
		)
		# 设置窗口大小，位置
		[Win32API.Auto]::RunMoveWindow($Process.MainWindowHandle, 1000 ,0, 920, 1080)
	}

	# 守护
	while(1) {
		# 捕捉错误
		Try
		{
			# 检查进程是否存在
			Get-Process virtualbox -ErrorAction Stop  
		}
	   catch 
		{
			# 启动进程
			Start-Process "C:\Program Files\Oracle\VirtualBox\VirtualBox.exe" -ArgumentList "-startvm Linux --fullscreen"
		}
		# 捕捉错误
		Try  
		{
			# 检查进程是否存在
			Get-Process notepad -ErrorAction Stop
		}  
		catch  
		{  
			# 启动进程
			Start-Process notepad
		}
		# 设置窗口大小及位置
		Get-Process notepad | Set-Start
		# 休息一下
		Start-Sleep -Seconds 1
	}

>

---

>

	powershell -ExecutionPolicy Bypass 

>

