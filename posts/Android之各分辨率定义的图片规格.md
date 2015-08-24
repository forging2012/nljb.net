---
title: Android之各分辨率定义的图片规格
date: '2015-07-21'
description:
categories:

tags:android

---

>

各种规格总结

>

---

>

	// 首先阐释一些术语和概念

	屏幕尺寸（screen size）: 屏幕的对角线测量。
		为了方便，Android把所有的屏幕尺寸分为了4个广义的大小：小、正常、大、更大

	屏幕密度（screen density）: 屏幕占据的物理区域所含像素的个数
		通常被称为dpi（dots per inch）即每英寸的像素点数

	分辨率（resolution）: 屏幕上物理像素的点数
		例如，有一个240px*400px的屏幕，可以理解为在这个屏幕上横着有400条线，每条线上有240个像素点

	像素（px）: 屏幕上的点

	dip(dp)：Density-independent pixel--->与密度无关的像素（下面将详细讲解）

>

---

>

由于JPG容易失真, 在Android开发中尽量避免使用.jpg图片, 应该使用.png图片, 它采用了从LZ77派生的无损数据压缩算法.

---

>

### 补充

>

	屏幕：尺寸5.1，分辨率1920X1080
	DPI：1920^2+1080^2(v)/5.1=2202/5.1=431

	mdpi		120dpi ~ 160dpi
	hdpi		160dpi ~ 240dpi
	xhdpi		240dpi ~ 320dpi
	xxhdpi	320dpi ~ 480dpi
	xxxhdpi	480dpi ~ 640dpi

	在设计图标时：
	对于五种主流像素密度（MDIP, HDPI, XHDPI, XXHDPI, XXXHDPI) 
	应按照(2:3:4:6:8)的比例进行缩放，(1x, 1.5x, 2x, 3x, 4x)

	例如：
	尺寸为48x48dp的图标，表示在MDPI的屏幕上其尺寸应为48x48px
	在HDPI的屏幕上其实际大小是MDPI的1.5倍（72x72px）… 依此类推

	字体：
	对于字体的使用，官方不建议使用低于12号字体大小值

	图片内存占用：
	drawable-xxhdpi	11.65M
	drawable		74.97M
	drawable-mhdpi	74.95M
	drawable-hdpi	35.38M
	在图片密度不匹配的情况下，低分辨率图片会被拉伸到高分辨率图片

>

---

>

### 密度陷阱

>

	虽然说dp可以去除不同像素密度的问题，使得1dp在不同像素密度上面的显示效果相同，但是还是由于Android屏幕设备的多样性

	如果使用dp来作为度量单位，并不是所有的屏幕的宽度都是相同的dp长度

	比如说:

		Nexus S和Nexus One	属于hdpi，		屏幕宽度是320dp
		Nexus 5				属于xxhdpi，	屏幕宽度是360dp
		Galaxy Nexus		属于xhdpi，		屏幕宽度是384dp
		Nexus 6 			属于xxxhdpi，	屏幕宽度是410dp

	所以说，光Google自己一家的产品就已经有这么多的标准，而且屏幕宽度和像素密度没有任何关联关系

	即使我们使用dp，在320dp宽度的设备和410dp的设备上，还是会有90dp的差别。

	当然，我们尽量使用match_parent和wrap_content，尽可能少的用dp来指定控件的具体长宽
		再结合上权重，大部分的情况我们都是可以做到适配的。

>

	此外还有一个解决办法，也就是指定所有分辨率密度对应关系

	// 下面是生成的一个320*480分辨率的文件
	// 因为宽高分割之后总分数和像素数相同
	// 所以x1就是1px，以此类推
	<?xml version="1.0" encoding="utf-8"?>
	<resources><dimen name="x1">1.0px</dimen>
	<dimen name="x2">2.0px</dimen>
	<dimen name="x3">3.0px</dimen>
	<dimen name="x4">4.0px</dimen>
	<dimen name="x5">5.0px</dimen>
	<dimen name="x6">6.0px</dimen>
	<dimen name="x7">7.0px</dimen>
	<dimen name="x8">8.0px</dimen>
	<dimen name="x9">9.0px</dimen>
	<dimen name="x10">10.0px</dimen>
	...省略好多行
	<dimen name="x300">300.0px</dimen>
	<dimen name="x301">301.0px</dimen>
	<dimen name="x302">302.0px</dimen>
	<dimen name="x303">303.0px</dimen>
	<dimen name="x304">304.0px</dimen>
	<dimen name="x305">305.0px</dimen>
	<dimen name="x306">306.0px</dimen>
	<dimen name="x307">307.0px</dimen>
	<dimen name="x308">308.0px</dimen>
	<dimen name="x309">309.0px</dimen>
	<dimen name="x310">310.0px</dimen>
	<dimen name="x311">311.0px</dimen>
	<dimen name="x312">312.0px</dimen>
	<dimen name="x313">313.0px</dimen>
	<dimen name="x314">314.0px</dimen>
	<dimen name="x315">315.0px</dimen>
	<dimen name="x316">316.0px</dimen>
	<dimen name="x317">317.0px</dimen>
	<dimen name="x318">318.0px</dimen>
	<dimen name="x319">319.0px</dimen>
	<dimen name="x320">320px</dimen>
	</resources>

	// 那么1080*1960分辨率下是什么样子呢？
	// 我们可以看下，由于1080和320是3.37倍的关系
	// 所以x1=3.37px
	<?xml version="1.0" encoding="utf-8"?>
	<resources><dimen name="x1">3.37px</dimen>
	<dimen name="x2">6.75px</dimen>
	<dimen name="x3">10.12px</dimen>
	<dimen name="x4">13.5px</dimen>
	<dimen name="x5">16.87px</dimen>
	<dimen name="x6">20.25px</dimen>
	<dimen name="x7">23.62px</dimen>
	<dimen name="x8">27.0px</dimen>
	<dimen name="x9">30.37px</dimen>
	<dimen name="x10">33.75px</dimen>
	...省略好多行
	<dimen name="x300">1012.5px</dimen>
	<dimen name="x301">1015.87px</dimen>
	<dimen name="x302">1019.25px</dimen>
	<dimen name="x303">1022.62px</dimen>
	<dimen name="x304">1026.0px</dimen>
	<dimen name="x305">1029.37px</dimen>
	<dimen name="x306">1032.75px</dimen>
	<dimen name="x307">1036.12px</dimen>
	<dimen name="x308">1039.5px</dimen>
	<dimen name="x309">1042.87px</dimen>
	<dimen name="x310">1046.25px</dimen>
	<dimen name="x311">1049.62px</dimen>
	<dimen name="x312">1053.0px</dimen>
	<dimen name="x313">1056.37px</dimen>
	<dimen name="x314">1059.75px</dimen>
	<dimen name="x315">1063.12px</dimen>
	<dimen name="x316">1066.5px</dimen>
	<dimen name="x317">1069.87px</dimen>
	<dimen name="x318">1073.25px</dimen>
	<dimen name="x319">1076.62px</dimen>
	<dimen name="x320">1080px</dimen>
	</resources>

>

	import java.io.File;
	import java.io.FileNotFoundException;
	import java.io.FileOutputStream;
	import java.io.PrintWriter;

	public class MakeXml {

		private final static String rootPath = "C:\\Users\\Administrator\\Desktop\\layoutroot\\values-{0}x{1}\\";

		private final static float dw = 320f;
		private final static float dh = 480f;

		private final static String WTemplate = "<dimen name=\"x{0}\">{1}px</dimen>\n";
		private final static String HTemplate = "<dimen name=\"y{0}\">{1}px</dimen>\n";

		public static void main(String[] args) {
			makeString(320, 480);
			makeString(480,800);
			makeString(480, 854);
			makeString(540, 960);
			makeString(600, 1024);
			makeString(720, 1184);
			makeString(720, 1196);
			makeString(720, 1280);
			makeString(768, 1024);
			makeString(800, 1280);
			makeString(1080, 1812);
			makeString(1080, 1920);
			makeString(1440, 2560);
		}

		public static void makeString(int w, int h) {

			StringBuffer sb = new StringBuffer();
			sb.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>\n");
			sb.append("<resources>");
			float cellw = w / dw;
			for (int i = 1; i < 320; i++) {
				sb.append(WTemplate.replace("{0}", i + "").replace("{1}",
						change(cellw * i) + ""));
			}
			sb.append(WTemplate.replace("{0}", "320").replace("{1}", w + ""));
			sb.append("</resources>");

			StringBuffer sb2 = new StringBuffer();
			sb2.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>\n");
			sb2.append("<resources>");
			float cellh = h / dh;
			for (int i = 1; i < 480; i++) {
				sb2.append(HTemplate.replace("{0}", i + "").replace("{1}",
						change(cellh * i) + ""));
			}
			sb2.append(HTemplate.replace("{0}", "480").replace("{1}", h + ""));
			sb2.append("</resources>");

			String path = rootPath.replace("{0}", h + "").replace("{1}", w + "");
			File rootFile = new File(path);
			if (!rootFile.exists()) {
				rootFile.mkdirs();
			}
			File layxFile = new File(path + "lay_x.xml");
			File layyFile = new File(path + "lay_y.xml");
			try {
				PrintWriter pw = new PrintWriter(new FileOutputStream(layxFile));
				pw.print(sb.toString());
				pw.close();
				pw = new PrintWriter(new FileOutputStream(layyFile));
				pw.print(sb2.toString());
				pw.close();
			} catch (FileNotFoundException e) {
				e.printStackTrace();
			}

		}

		public static float change(float a) {
			int temp = (int) (a * 100);
			return temp / 100f;
		}
	}

>

---

>

应用程序图标 （Icon）应当是一个 Alpha 通道透明的32位 PNG 图片。

>

	由于安卓设备众多，一个应用程序图标需要设计几种不同大小，如：
	LDPI (Low Density Screen，120 DPI)，其图标大小为 36 x 36 px。
	MDPI (Medium Density Screen, 160 DPI)，其图标大小为 48 x 48 px。
	HDPI (High Density Screen, 240 DPI)，其图标大小为 72 x 72 px。
	xhdpi (Extra-high density screen, 320 DPI)，其图标大小为 96 x 96 px。

	建议在设计过程中，在四周空出几个像素点使得设计的图标与其他图标在视觉上一致，例如，
	96 x 96 px 图标可以画图区域大小可以设为 88 x 88 px， 四周留出4个像素用于填充（无底色）。
	72 x 72 px 图标可以画图区域大小可以设为 68 x 68 px， 四周留出2个像素用于填充（无底色）。
	48 x 48 px 图标可以画图区域大小可以设为 46 x 46 px， 四周留出1个像素用于填充（无底色）。
	36 x 36 px 图标可以画图区域大小可以设为 34 x 34 px， 四周留出1个像素用于填充（无底色）。

>

---

>

	每英寸像素数, 可以反映屏幕的清晰度，用于缩放UI的
	ldpi (low) 							~120dpi
	mdpi (medium) 						~160dpi
	hdpi (high) 						~240dpi
	xhdpi (extra-high) 					~320dpi
	xxhdpi (extra-extra-high) 			~480dpi
	xxxhdpi (extra-extra-extra-high) 	~640dpi

>

    drawable-ldpi   放低分辨率的图片，即QVGA(240×320)
    drawable-mdpi   放中分辨率的图片，即HVGA(320×480)
    drawable-hdpi   放高分辨率的图片，如WVGA(480x800),FWVGA (480x854)。
    drawable-xhdpi  放高分辨率的图片，即720p(1280×720)
    drawable-xxhdpi 放高分辨率的图片，即1080p(1920×1080)

>

---

>

Android设备屏幕尺寸分布

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/3.png" alt="" width="500" hight="200" >

>

	上图可以看出

	对应normal尺寸的屏幕范围集中在常见的3到5寸屏之间
		large尺寸对应的就主要是5到7寸的nottpad之类的设备，例如三星的Note和Nexus7平板等

	接下来是屏幕密度（dpi），需要说明的是，平时所说的屏幕分辨率其实不能作为屏幕适配的依据
		 应该依据屏幕密度和屏幕尺寸来换算，屏幕密度是指每英寸屏幕内容纳的像素数

	屏幕密度从ldpi到xhdpi分别对应为120dpi、160dpi、240dpi、320dpi
		屏幕密度越高、分辨率越高、屏幕尺寸越小就产生了视网膜屏幕。

>

---

>

DIP单位详解

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/4.jpeg" alt="" width="200" hight="200" >

>

	Android规定一个dip的大小相当于160dpi屏幕上的一个像素
		它是系统为“中等的”密度屏设定的基准密度，在不同dpi屏幕上dp对应的像素数是不同的

	需要时，基于当前屏的实际密度，系统会透明地放缩dip单。
		dip单位根据公式像素值 = [dip*(dpi/160)](px)（其中px是单位）转化为屏幕像素。
		根据此公式可以计算出一个dip分别在120dpi、160dpi、240dpi、320dpi
		屏幕中对应的像素数分别为0.75、1、1.5、2.0，比例为3:4:6:8。
		因此，在不同屏幕密度上，以mdpi作为基准，对位图进行3:4:6:8比例的放缩会达到适配的效果。

>

	dip与一般的px不太一样，它是独立于屏幕密度的。什么是独立于密度？

	先来说下一般的px，如果将一个相同长宽像素的图片放在不同屏幕密度大小的屏幕中
		那么，在低密度屏幕中图片会显示的很大，在高密度屏幕中则会显示的很小；
		但是，如果使用dip为单位的图片显示的效果则是，屏幕密度越大的手机，图片显示的像素也相应增大
		这样在屏幕密度大的手机和屏幕密度小的手机上，图片看上去大小基本相同。
		有了上文对dip的讲解，是否对这个现象有所理解呢？

>

举个例子来说一下：

>

	现在有三个物理长宽分别为3寸、4寸，屏幕密度分别为120dpi、160dpi、240dpi的手机
		则三个屏幕的分辨率分别为360px*480px、480px*640px、

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/5.jpeg" alt="" width="600">

>

	将三个手机屏幕的宽分为三等份，则根据dpi的定义，三个屏幕中每等份分别容纳120px、160px、240px。
		现在假设有一个控件imageview 它的长宽分别为160px、160px，还有一个160px*160px的图片资源
		当程序运行时，该图片在三个屏幕上会呈现以下效果：

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/6.png" alt="" width="550" hight="250" >

>

	如果将imageview的长宽分别改为160dip、160dip，图片将在三个屏幕上呈现以下效果:

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/7.png" alt="" width="550" hight="250" >

>

	上文提到在这三种屏幕密度下一个dip分别对应0.75px、1px、1.5px
		所以在三种屏幕上该图片占据120px、160px、240px,各自占屏幕的三分之一，所以看起来是一样大的。

>

	由上文可总结出Android在适配不同屏幕密度时，可以用dip作为控件的单位，视情况放缩dip单位。

	当应用没有指出图片对应的控件的大小，Android是如何让图片适配不同屏幕的呢？

>

	在Android2.1之前，开发应用时只有一个放图片资源的drawable文件夹，这样程序在不同屏幕密度的手机上运行时
		系统只会从drawable这个文件夹下调图片资源，并且系统会默认认为这个文件夹下的所有资源是为mdpi屏幕提供的
		所以在hdpi屏幕上系统会按比例将drawable下的图片扩大为原来的1.5倍
		在ldpi屏幕上系统会按比例将drawable下的图片缩小为原来的0.75倍
		这样会大大降低页面效果。

	在Android2.1以及之后，出现了drawable-ldpi、drawable-mdpi、drawable-hdpi、drawable-xhdpi、drawable-xxhdpi。
		在这些文件下提供的图片大小最好是3:4:6:8:12。
		程序在不同的屏幕密度下运行时，会首先去符合当前屏幕密度的文件夹下找对应的资源
		如果没有，系统会以最省力为前提去别的文件夹下找对应的资源并对其进行相应的缩放
		如果还没有，就回去默认的drawable文件夹下找，然后按照2.1之前的规则缩放
		如果还没有找到，应用就会报错或者直接crash掉了。

>

举个例子

>

	现在有一个ldpi的手机屏幕，有一个应用在其上运行(假如只有ldpi、mdpi、hdpi还有drawable四个存放图片的文件夹)
		并需要调用一个图片a.png（在下文中用a来代替a.png）。Android系统会经历以下流程：

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/8.png" alt="" width="600" hight="700" >

>

	将hdpi中的图片大小缩小为原来的一半相比将mdpi中的图片大小缩小为原来的3/4，计算机要省力，只需进行简单地右移一位操作。
		所以系统在ldpi下找不到a的时候会首先去hdpi下去找。当存在xhdpi、xxhdpi时，系统会按相同的规则去调用资源。

       Drawable-ldpi 3、Drawable-mdpi  4、Drawable-hdpi  6中的3、4、6指的是同一个图片在三个文件夹下的大小之比。

>

	Android开发者在做图片适配时需要注意一下两点

	盛放图片的控件要用dip单位来定义其长宽。

	最好在ldpi、mdpi、hdpi、xhdpi、xxhdpi文件夹下提供大小比例为3:4:6:8:12的图片。
		当然如果有质量好的.9.png图片的话，提供一个也可以。

>

---

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/1.gif" alt="" width="500" hight="300" >

>

---

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/2.gif" alt="" width="600" hight="250" >

>
