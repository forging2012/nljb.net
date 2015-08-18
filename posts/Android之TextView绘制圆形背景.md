---
title: Android之TextView绘制圆形背景
date: '2015-08-18'
description:
categories:

tags:android

---

>

### TextView Draw Circle Background 

>

	public class CircleView extends TextView {

		private Paint mBgPaint = new Paint();

		PaintFlagsDrawFilter pfd = new PaintFlagsDrawFilter(0, Paint.ANTI_ALIAS_FLAG | Paint.FILTER_BITMAP_FLAG);

		public CircleView(Context context, AttributeSet attrs, int defStyle) {
			super(context, attrs, defStyle);
			// TODO Auto-generated constructor stub
		}

		public CircleView(Context context, AttributeSet attrs) {
			super(context, attrs);
			// TODO Auto-generated constructor stub
			mBgPaint.setColor(Color.WHITE);
			mBgPaint.setAntiAlias(true);
		}

		public CircleView(Context context) {
			super(context);
			// TODO Auto-generated constructor stub
			mBgPaint.setColor(Color.WHITE);
			mBgPaint.setAntiAlias(true);
		}

		@Override
		protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
			// TODO Auto-generated method stub
			super.onMeasure(widthMeasureSpec, heightMeasureSpec);
			int measuredWidth = getMeasuredWidth();
			int measuredHeight = getMeasuredHeight();
			int max = Math.max(measuredWidth, measuredHeight);
			setMeasuredDimension(max, max);
		}

		@Override
		public void setBackgroundColor(int color) {
			// TODO Auto-generated method stub
			mBgPaint.setColor(color);
		}

		@Override
		public void draw(Canvas canvas) {
			// TODO Auto-generated method stub
			canvas.setDrawFilter(pfd);
			canvas.drawCircle(getWidth() / 2, getHeight() / 2, Math.max(getWidth(), getHeight()) / 4, mBgPaint);
			super.draw(canvas);
		}
	}


