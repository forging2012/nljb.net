---
title: Android之Animation的使用方法
date: '2015-08-24'
description:
categories:

tags:android

---

>

### Animation

>

Android支持两种动画模式: tween animation, frame animation

>

在android 3.0中又引入了一个新的动画系统：property animation

>

这三种动画模式在SDK中被称为property animation,view animation,drawable animation。

>

可通过NineOldAndroids项目在3.0之前的系统中使用Property Animation

>

---

>

###  View Animation（Tween Animation）

>

	View Animation（Tween Animation）：
		补间动画，给出两个关键帧，通过一些算法将给定属性值在给定的时间内在两个关键帧间渐变。

	View animation只能应用于View对象，而且只支持一部分属性，如支持缩放旋转而不支持背景颜色的改变。

	而且对于View animation，它只是改变了View对象绘制的位置，而没有改变View对象本身
		比如，你有一个Button，坐标（100,100），Width:200,Height:50
		而你有一个动画使其变为Width：100，Height：100
		你会发现动画过程中触发按钮点击的区域仍是(100,100)-(300,150)。

	View Animation就是一系列View形状的变换，如大小的缩放，透明度的改变，位置的改变
		动画的定义既可以用代码定义也可以用XML定义，当然，建议用XML定义。

	可以给一个View同时设置多个动画，比如从透明至不透明的淡入效果，与从小到大的放大效果
		这些动画可以同时进行，也可以在一个完成之后开始另一个。

	用XML定义的动画放在/res/anim/文件夹内
		XML文件的根元素可以为<alpha>,<scale>,<translate>,<rotate>,interpolator元素或<set>
		(表示以上几个动画的集合，set可以嵌套)。
		默认情况下，所有动画是同时进行的，可以通过startOffset属性设置各个动画的开始偏移
		（开始时间）来达到动画顺序播放的效果。

	可以通过设置interpolator属性改变动画渐变的方式
		如AccelerateInterpolator，开始时慢，然后逐渐加快。默认为AccelerateDecelerateInterpolator。

	定义好动画的XML文件后，可以通过类似下面的代码对指定View应用动画。

>

	ImageView spaceshipImage = (ImageView)findViewById(R.id.spaceshipImage);
	Animation hyperspaceJumpAnimation=AnimationUtils.loadAnimation(this, R.anim.hyperspace_jump);
	spaceshipImage.startAnimation(hyperspaceJumpAnimation);

>

---

>

### Drawable Animation（Frame Animation）

>

	Drawable Animation（Frame Animation）：帧动画，就像GIF图片
		通过一系列Drawable依次显示来模拟动画的效果

	在XML中的定义方式如下：
	<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
		android:oneshot="true">
		<item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
		<item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
		<item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
	</animation-list>

	必须以<animation-list>为根元素，以<item>表示要轮换显示的图片，duration属性表示各项显示的时间。
		XML文件要放在/res/drawable/目录下。

	示例：
	protected void onCreate(Bundle savedInstanceState) {
        // TODO Auto-generated method stub
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        imageView = (ImageView) findViewById(R.id.imageView1);
        imageView.setBackgroundResource(R.drawable.drawable_anim);
        anim = (AnimationDrawable) imageView.getBackground();
    }

    public boolean onTouchEvent(MotionEvent event) {
        if (event.getAction() == MotionEvent.ACTION_DOWN) {
            anim.stop();
            anim.start();
            return true;
        }
        return super.onTouchEvent(event);
    }

	在实验中遇到问题(转)：
		要在代码中调用Imageview的setBackgroundResource方法，如果直接在XML布局文件中设置其src属性当触发动画时会FC。
		在动画start()之前要先stop()，不然在第一次动画之后会停在最后一帧，这样动画就只会触发一次。
		最后一点是SDK中提到的，不要在onCreate中调用start，因为AnimationDrawable还没有完全跟Window相关联
		如果想要界面显示时就开始动画的话，可以在onWindowFoucsChanged()中调用start()。

>

---

>

### Property Animation

>

	属性动画，这个是在Android 3.0中才引进的，以前学WPF时里面的动画机制好像就是这个
		它更改的是对象的实际属性，在（Tween Animation）中，其改变的是View的绘制效果，真正的View的属性保持不变
		比如无论你在对话中如何缩放Button的大小，Button的有效点击区域还是没有应用动画时的区域，其位置与大小都不变。
		而在Property Animation中，改变的是对象的实际属性，如Button的缩放，Button的位置与大小属性值都改变了。
		而且Property Animation不止可以应用于View，还可以应用于任何对象。
		Property Animation只是表示一个值在一段时间内的改变，当值改变时要做什么事情完全是你自己决定的。

	在Property Animation中，可以对动画应用以下属性：

	Duration：
		动画的持续时间
	TimeInterpolation：
		属性值的计算方式，如先快后慢
	TypeEvaluator：
		根据属性的开始、结束值与TimeInterpolation计算出的因子计算出当前时间的属性值
	Repeat Count and behavoir：
		重复次数与方式，如播放3次、5次、无限循环，可以此动画一直重复，或播放完时再反向播放
	Animation sets：
		动画集合，即可以同时对一个对象应用几个动画，这些动画可以同时播放也可以对不同动画设置不同开始偏移
	Frame refreash delay：
		多少时间刷新一次，即每隔多少时间计算一次属性值，默认为10ms，最终刷新时间还受系统进程调度与硬件的影响

>

Property Animation的工作方式

>

	对于下图的动画，这个对象的X坐标在40ms内从0移动到40 pixel

	按默认的10ms刷新一次，这个对象会移动4次，每次移动40/4=10pixel。

>

<img src="{{urls.media}}/Android之Animation的使用方法/1.png" alt="" width="560" hight="160">

>

	也可以改变属性值的改变方法，即设置不同的interpolation，在下图中运动速度先逐渐增大再逐渐减小

>

<img src="{{urls.media}}/Android之Animation的使用方法/2.png" alt="" width="560" hight="160">

>

	下图显示了与上述动画相关的关键对象

>

<img src="{{urls.media}}/Android之Animation的使用方法/3.png" alt="" width="720" hight="240">

>

	ValueAnimator  表示一个动画，包含动画的开始值，结束值，持续时间等属性。
	ValueAnimator封装了一个TimeInterpolator，TimeInterpolator定义了属性值在开始值与结束值之间的插值方法。
	ValueAnimator还封装了一个TypeAnimator，根据开始、结束值与TimeIniterpolator计算得到的值计算出属性值。
	ValueAnimator根据动画已进行的时间跟动画总时间（duration）的比计算出一个时间因子（0~1）

>

---

>

### ValueAnimator

>

ValueAnimator包含了Property Animation动画的所有核心功能，如动画时间，开始、结束属性值，相应时间属性值计算方法等。

>

	在我看来，使用 ValueAnimator 只是为我们创建了一个过程
		我们可以用ValueAnimator.ofXXX()进行创建
		然后通过各种setXXX()给其设定过程的时间，速率变化效果等
		然后通过addUpdateListener()中去拿这个过程中回调回来的中间值
		然后使用这些中间值改变view的属性形成动态效果；

>

	比如我们使用 ValueAnimator 在2S内将view横向拉长为2倍，纵向压缩为0：

	// 在2S内将view横向拉长为2倍，纵向压缩为0
	// 创建0－1的一个过程,任何复杂的过程都可以采用归一化，然后在addUpdateListener回调里去做自己想要的变化
	ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, 1);
	// 设置过程的时间为2S
	valueAnimator.setDuration(SCALE_ANIM_TIME);
	// 设置这个过程是速度不断变快的
	valueAnimator.setInterpolator(new AccelerateInterpolator());
	// 这个过程中不断执行的回调
	valueAnimator.addUpdateListener(new AnimatorUpdateListener() {
		@Override
		public void onAnimationUpdate(ValueAnimator animation) {
			// 不断回调的在0-1这个范围内，经过插值器插值之后的返回值
			float value = (Float) animation.getAnimatedValue();
			// ViewHelper可直接用于修改view属性
			// 将宽在2S内放大一倍
			ViewHelper.setScaleX(mTestImage, 1 + value);
			// 将高在2S内压缩为0
			ViewHelper.setScaleY(mTestImage, 1 - value);
		}
	});
	// AnimatorListenerAdapter是AnimatorListener的空实现，根据需要覆盖的方法自行选择
	valueAnimator.addListener(new AnimatorListenerAdapter() {
		@Override
		public void onAnimationStart(Animator animation) {
			super.onAnimationStart(animation);
			Toast.makeText(getApplicationContext(), "onAnimationStart", Toast.LENGTH_SHORT)
					.show();
		}

		@Override
		public void onAnimationEnd(Animator animation) {
			super.onAnimationEnd(animation);
			Toast.makeText(getApplicationContext(), "onAnimationEnd", Toast.LENGTH_SHORT)
					.show();
		}

		@Override
		public void onAnimationCancel(Animator animation) {
			super.onAnimationCancel(animation);
		}

		@Override
		public void onAnimationRepeat(Animator animation) {
			super.onAnimationRepeat(animation);
		}
	});
	valueAnimator.start();

>

	动画其实就是在一个时间段内不断去改变view的一些属性值
		这些属性值动态变化，不断重绘的过程，也就形成了我们所看到的动效；

>

	那么基于此，我们可以知道这个过程都是通过时间来控制的，时间走过的比例肯定在 0-1 之间
		如果我们直接用这个走过的时间比例去算当前属性值，那么整个过程则是匀速（线性）的；

>

	横轴就是经过的时间比例，肯定是匀速的从0-1，纵轴则是时间比例经过加工后的插值
		这个对应过程则是Interpolator（插值器）对应的过程；

	减速线则对应DecelerateInterpolater,因为它的斜率越来越平，所以瞬时速度越来越小，则形成了减速效果；

	其他的效果类似，目前android里提供的插值器有如下一些：

	AccelerateInterpolator　　　　　     
		加速，开始时慢中间加速
	DecelerateInterpolator　　　 　　   
		减速，开始时快然后减速
	AccelerateDecelerateInterolator　   
		先加速后减速，开始结束时慢，中间加速
	AnticipateInterpolator　　　　　　  
		反向 ，先向相反方向改变一段再加速播放
	AnticipateOvershootInterpolator　   
		反向加回弹，先向相反方向改变，再加速播放，会超出目的值然后缓慢移动至目的值
	BounceInterpolator　　　　　　　  
		跳跃，快到目的值时值会跳跃，如目的值100，后面的值可能依次为85，77，70，80，90，100
	CycleIinterpolator　　　　　　　　 
		循环，动画循环一定次数，值的改变为一正弦函数：Math.sin(2 * mCycles * Math.PI * input)
	LinearInterpolator　　　　　　　　 
		线性，线性均匀改变
	OvershottInterpolator　　　　　　  
		回弹，最后超出目的值然后缓慢改变到目的值
	TimeInterpolator　　　　　　　　   
		一个接口，允许你自定义interpolator，以上几个都是实现了这个接口

	其实想实现对应的效果，其实是找一条曲线对对应条件进行模拟
		然后根据曲线函数，和X值，得出每个时间点上对应的Y值（插值），这也就是插值器原理；

>

---

>

### ObjectAnimator

>

	bjectAnimator 是ValueAnimator 的子类，可以直接改变Object的属性，目前可供改变的属性主要有：

	translationX, translationY           
		View相对于原始位置的偏移量
	rotation, rotationX, rotationY       
		旋转，rotation用于2D旋转角度，3D中用到后两个
	scaleX, scaleY                           
		缩放比
	x, y                                             
		View的最终坐标，是View的left，top位置加上translationX，translationY
	alpha                                          
		透明度

	关于Animator主要分享这两个类，其他的比如 PropertyValuesHolder 大家可以自己去看；

>

