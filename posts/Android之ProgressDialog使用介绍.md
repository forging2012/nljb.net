---
title: Android之ProgressDialog使用介绍
date: '2015-11-02'
description:
categories:

tags:android

---

>

### ProgressDialog

>

ProgressDialog为进度对话框

>

ProgressDialog的样式有两种，一种是圆形不明确状态，一种是水平进度条状态

>

	// 设置进度条的形式为圆形转动的进度条
	dialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
	// 设置是否可以通过点击Back键取消 
	dialog.setCancelable(true);
	// 设置在点击Dialog外是否取消Dialog进度条
	dialog.setCanceledOnTouchOutside(false);

>

---

>

	// ProgressDialog
	private ProgressDialog mDialog;

	// ...
	mDialog = new ProgressDialog(PreviewActivity.this);
	mDialog.setCanceledOnTouchOutside(false);
	mDialog.setCancelable(false);
	mDialog.setMessage("请稍后 ...");
	mDialog.show();

>

