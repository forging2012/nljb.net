---
title: Android之Ripple按钮水波纹效果
date: '2015-04-29'
description:
categories:

tags:android

---

>

### Ripple

>

注：代码摘抄自网络

>

	// 在Layout中使用水波纹效果的View对象
	<com.example.nljb.surpass.RippleImageView
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	android:background="@drawable/guide_0" />

>

	// 集成需要实现水波纹效果的View对象类(ImageButton)
	public class RippleImageButton extends ImageButton {

	    // 起始点
	    private int mInitX;
	    private int mInitY;

	    // 高度和宽度
	    private int mWidth;
	    private int mHeight;

	    // 绘制的半径
	    private int mRadius;
	    private int mStep;
	    private int mDrawRadius;

	    private boolean mDrawFinish;

	    private boolean mDrawBack;

	    private final int DURATION = 240;
	    private final int FREQUENCY = 10;
	    private int mCycle;
	    private final Rect mRect = new Rect();

	    private Paint mRevealPaint = new Paint(Paint.ANTI_ALIAS_FLAG);

	    public RippleImageButton(Context context) {
		super(context);
		initView(context);
	    }

	    public RippleImageButton(Context context, AttributeSet attrs) {
		super(context, attrs);
		initView(context);
	    }

	    public RippleImageButton(Context context, AttributeSet attrs, int defStyleAttr) {
		super(context, attrs, defStyleAttr);
		initView(context);
	    }

	    private void initView(Context context) {
		mRevealPaint.setColor(0x10000000);
		mCycle = DURATION / FREQUENCY;
		mDrawFinish = true;
	    }

	    @Override
	    protected void onDraw(Canvas canvas) {
		if (mDrawFinish) {
		    super.onDraw(canvas);
		    return;
		}
		canvas.drawColor(0x08000000);
		super.onDraw(canvas);
		if (mStep == 0) {
		    return;
		}
		mDrawRadius = mDrawRadius + mStep;


		if (mDrawRadius > mRadius) {
		    mDrawRadius = 0;
		    canvas.drawCircle(mInitX, mInitY, mRadius, mRevealPaint);
		    mDrawFinish = true;
		    invalidate();
		    return;
		}

		canvas.drawCircle(mInitX, mInitY, mDrawRadius, mRevealPaint);
		invalidate();
	    }

	    @Override
	    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		super.onMeasure(widthMeasureSpec, heightMeasureSpec);
		mRect.set(0, 0, getMeasuredWidth(), getMeasuredHeight());
	    }

	    private void updateDrawData() {
		int radiusLeftTop = (int) Math.sqrt((mRect.left - mInitX) * (mRect.left - mInitX) +
			(mRect.top - mInitY) * (mRect.top - mInitY));
		int radiusRightTop = (int) Math.sqrt((mRect.right - mInitX) * (mRect.right - mInitX) +
			(mRect.top - mInitY) * (mRect.top - mInitY));
		int radiusLeftBottom = (int) Math.sqrt((mRect.left - mInitX) * (mRect.left - mInitX) +
			(mRect.bottom - mInitY) * (mRect.bottom - mInitY));
		int radiusRightBottom = (int) Math.sqrt((mRect.right - mInitX) * (mRect.right - mInitX) +
			(mRect.bottom - mInitY) * (mRect.bottom - mInitY));
		mRadius = getMax(radiusLeftTop, radiusRightTop, radiusLeftBottom, radiusRightBottom);
		mStep = mRadius / mCycle;
	    }

	    @Override
	    public boolean onTouchEvent(MotionEvent event) {
		final int action = MotionEventCompat.getActionMasked(event);
		switch (action) {
		    case MotionEvent.ACTION_DOWN: {
			mDrawFinish = false;
			int index = MotionEventCompat.getActionIndex(event);
			int eventId = MotionEventCompat.getPointerId(event, index);
			if (eventId != -1) {
			    mInitX = (int) MotionEventCompat.getX(event, index);
			    mInitY = (int) MotionEventCompat.getY(event, index);
			    updateDrawData();
			    invalidate();
			}
			break;
		    }
		    case MotionEvent.ACTION_CANCEL:
		    case MotionEvent.ACTION_UP:
			mStep = (int) (2.5f * mStep);
			mDrawBack = true;
			break;
		}
		return super.onTouchEvent(event);
	    }

	    private int getMax(int... radius) {
		if (radius.length == 0) {
		    return 0;
		}
		int max = radius[0];
		for (int m : radius) {
		    if (m > max) {
			max = m;
		    }
		}
		return max;
	    }

	}


