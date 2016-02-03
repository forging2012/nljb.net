---
title: Android之接口隔离原则
date: '2016-02-03'
description:
categories:

tags:android

---

>

### 接口隔离原则

>

	接口隔离原则：InterfaceSegregaiton Principles 缩写：ISP

	定义：
		客户端不应该依赖它不需要的接口
		类间的依赖关系应该建立在最小的接口上

	接口隔离原则说白了就是，让客户端依赖的接口尽可能地小

>

---

>

	// 例：
	public void put(String url, Bitmap bmp) {
		FileOutputStream fileOutputStream = null;
		try {
			fileOutputStream = new FileOutputStream(cacheDir + url);
			bmp.compress(CompressFormat.PNG, 100, fileOutputStream);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} finally {
			if (fileOutputStream != null) {
				try {
					fileOutputStream.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}

>

	// 按照接口隔离原则，修改如下
	
	public final class CloseUtils {
		public static void closeQuietly(Closeable closeable) {
			if (closeable != null) {
				try {
					closeable.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}

	public void put(String url, Bitmap bmp) {
		FileOutputStream fileOutputStream = null;
		try {
			fileOutputStream = new FileOutputStream(cacheDir + url);
			bmp.compress(CompressFormat.PNG, 100, fileOutputStream);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} finally {
			CloseUtils.closeQuietly(fileOutputStream);
		}
	}

>
	

