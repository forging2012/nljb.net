---
title: Android之表单验证框架
date: '2015-07-28'
description:
categories:

tags:android

---

>

### Android Saripaar

>

	下载地址：https://github.com/ragunathjawahar/android-saripaar

>

	Saripaar特性：

		使用Annotation标注的生命性规则
		可扩展性
		支持同步/异步验证，无线担心线程问题
		使用简单，只需下载 jar包，放到项目的libs目录下即可
		使用规则来隔离验证逻辑
		兼容其他Annotation框架，例如  AndroidAnnotations, RoboGuice,

>

	// 使用android-saripaar提供的Annotation来标注验证规则

	@Required(order = 1)
	@Email(order = 2)
	private EditText emailEditText;

	@Password(order = 3)
	@TextRule(order = 4, minLength = 6, message = "Enter at least 6 characters.")
	private EditText passwordEditText;

	@ConfirmPassword(order = 5)
	private EditText confirmPasswordEditText;

	@Checked(order = 6, message = "You must agree to the terms.")
	private CheckBox iAgreeCheckBox;

	// 每个规则都是顾名思义的，其中 order 属性 是必须的，用来告诉Saripaar这些输入规则的验证顺序。

>

	// 初始化一个Validator
	public void onCreate() {
		super.onCreate();
		// Code…

		validator = new Validator(this);
		validator.setValidationListener(this);

		// More code…
	}

	// 需要一个Validator 和 ValidationListener。后者用来接收验证结果通知。

>

	// 实现一个ValidationListener
	public class RegistrationActivity implements ValidationListener {

		public void onValidationSucceeded() {
			Toast.makeText(this, "Yay! we got it right!", Toast.LENGTH_SHORT).show();
		}

		public void onValidationFailed(View failedView, Rule<?> failedRule) {
			String message = failedRule.getFailureMessage();

			if (failedView instanceof EditText) {
				failedView.requestFocus();
				((EditText) failedView).setError(message);
			} else {
				Toast.makeText(this, message, Toast.LENGTH_SHORT).show();
			}
		}

	}

	// onValidationSucceeded() - 当所有验证规则都通过时调用该函数
	// onValidationFailed(View, Rule<?>) - 当验证失败时调用该函数，View为失败的控件，Rule为具体的规则.

>

	// 发起验证 
	registerButton.setOnClickListener(new OnClickListener() {
		public void onClick(View v) {
			validator.validate();
		}
	});

	// Validator.validate() 发起验证，并通过 ValidationListener 来通知验证结果。
	// 调用函数 Validator.validateAsync()  可以在一个AsyncTask中发起验证。


