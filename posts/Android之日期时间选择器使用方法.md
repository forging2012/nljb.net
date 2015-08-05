---
title: Android之日期时间选择器使用方法
date: '2015-08-05'
description:
categories:

tags:android

---

>

### 日期时间选择器

>

---

>

	// 设置日期
	@Override
	public void onClick(View v) {
		new DatePickerDialog(activity,
				new DatePickerDialog.OnDateSetListener() {
					@Override
					public void onDateSet(DatePicker view, int year,
										  int monthOfYear, int dayOfMonth) {
						// 设置
						calendar.set(year, monthOfYear, dayOfMonth);
					}
				}, // 设置年,月,日
				calendar.get(Calendar.YEAR),
				calendar.get(Calendar.MONTH),
				calendar.get(Calendar.DAY_OF_MONTH)).show();
	}

>

	// 设置时间
	public void onClick(View v) {
		new TimePickerDialog(activity,
				new TimePickerDialog.OnTimeSetListener() {
					@Override
					public void onTimeSet(TimePicker view,
										  int hourOfDay, int minute) {
						// 设置
						calendar.set(calendar.get(Calendar.YEAR),
								calendar.get(Calendar.MONTH),
								calendar.get(Calendar.DAY_OF_MONTH),
								hourOfDay, minute);
					}
				}, // 设置 时,分,是否是24小时制
				calendar.get(Calendar.HOUR_OF_DAY),
				calendar.get(Calendar.MINUTE), true).show();
	}

>

	// 获取日期与时间
	SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	format.format(calendar.getTime())

>

---

>

<img src="{{urls.media}}/Android之日期时间选择器使用方法/1.jpg" alt="" width="300" hight="600">

>


