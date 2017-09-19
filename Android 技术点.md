#Android 技术点

> Android一些技术点的零碎记忆



[TOC]

## 软键盘弹出状态

```
	<activity
            android:name=".login.activity.DVDLogin4SellerActivity"
            android:screenOrientation="portrait"
            android:windowSoftInputMode="stateHidden|adjustResize" />
```

活动的主窗口如何与包含屏幕上的软键盘窗口交互。这个属性的设置将会影响两件事情:

1. 软键盘的状态——是否它是隐藏或显示——当活动(Activity)成为用户关注的焦点。

2. 活动的主窗口调整——是否减少活动主窗口大小以便腾出空间放软键盘或是否当活动窗口的部分被软键盘覆盖时它的内容的当前焦点是可见的。

可选属性：
​	
* **stateUnspecified** 软键盘的状态(是否它是隐藏或可见)没有被指定。系统将选择一个合适的状态或依赖于主题的设置。这个是为了软件盘行为默认的设置。

* **stateUnchanged** 软键盘被保持无论它上次是什么状态，是否可见或隐藏，当主窗口出现在前面时。

* **stateHidden** 当用户选择该Activity时，软键盘被隐藏——也就是，当用户确定导航到该Activity时，而不是返回到它由于离开另一个Activity。

* **stateAlwaysHidden** 软键盘总是被隐藏的，当该Activity主窗口获取焦点时。

* **stateVisible** 软键盘是可见的，当那个是正常合适的时(当用户导航到Activity主窗口时)。

* **stateAlwaysVisible** 当用户选择这个Activity时，软键盘是可见的——也就是，也就是，当用户确定导航到该Activity时，而不是返回到它由于离开另一个Activity。
* **adjustUnspecified** 它不被指定是否该Activity主窗口调整大小以便留出软键盘的空间，或是否窗口上的内容得到屏幕上当前的焦点是可见的。系统将自动选择这些模式中一种主要依赖于是否窗口的内容有任何布局视图能够滚动他们的内容。如果有这样的一个视图，这个窗口将调整大小，这样的假设可以使滚动窗口的内容在一个较小的区域中可见的。这个是主窗口默认的行为设置。

* **adjustResize** 该Activity主窗口总是被调整屏幕的大小以便留出软键盘的空间。

* **adjustPan** 该Activity主窗口并不调整屏幕的大小以便留出软键盘的空间。相反，当前窗口的内容将自动移动以便当前焦点从不被键盘覆盖和用户能总是看到输入内容的部分。这个通常是不期望比调整大小，因为用户可能关闭软键盘以便获得与被覆盖内容的交互操作。

## emoji判断

```
private static final String EMOJI_REGEX = "([\\u20a0-\\u32ff\\ud83c\\udc00-\\ud83d\\udeff\\udbb9\\udce5-\\udbb9\\udcee])";

private boolean containsEmoji(CharSequence source) {
	Matcher matchEmo = Pattern.compile(EMOJI_REGEX).matcher(source);     
	return matchEmo.find();
}

```

## 唯一设备号

```
compile ('com.taobao.android:alisdk-hotfix:2.0.9') {
     exclude(module:'utdid4all')
}
```
utdid实际上是为设备生成唯一deviceid的一个基础类库

## 代理

*todo*

## RxAndroid 

*todo*


## 长期后台任务

Timer 和 AlarmManager



## File末尾追加内容

**方法一：**
~~~java
 try {
   // 打开一个随机访问文件流，按读写方式
   RandomAccessFile randomFile = new RandomAccessFile(fileName, "rw");
   // 文件长度，字节数
   long fileLength = randomFile.length();
   //将写文件指针移到文件尾。
   randomFile.seek(fileLength);
   randomFile.writeBytes(content);
   randomFile.close();
 } catch (IOException e) {
   e.printStackTrace();
 }
~~~

**方法二：**

~~~java
FileWriter fileWriter = null;
try {
  File logFile = getCrashLogFile();
  if (logFile != null) {
    fileWriter = new FileWriter(logFile, true);
    fileWriter.write(result);
    fileWriter.flush();
    fileWriter.close();
  }
} catch (Exception e) {
  Log.e(TAG, "an error occurred while writing file...", e);
} finally {
  if (fileWriter != null) {
    try {
      fileWriter.close();
    } catch (IOException e) {
      Log.e(TAG, "saveCrashInfo2File: ", e);
    }
  }
}
~~~



##location

~~~java
/**
 * 百度坐标（BD09）、国测局坐标（火星坐标，GCJ02）、和WGS84坐标系之间的转换的工具
 * 
 * 参考 https://github.com/wandergis/coordtransform 实现的Java版本
 * @author geosmart
 */
public class CoordinateTransformUtil {
	static double x_pi = 3.14159265358979324 * 3000.0 / 180.0;
	// π
	static double pi = 3.1415926535897932384626;
	// 长半轴
	static double a = 6378245.0;
	// 扁率
	static double ee = 0.00669342162296594323;

	/**
	 * 百度坐标系(BD-09)转WGS坐标
	 * 
	 * @param lng 百度坐标纬度
	 * @param lat 百度坐标经度
	 * @return WGS84坐标数组
	 */
	public static double[] bd09towgs84(double lng, double lat) {
		double[] gcj = bd09togcj02(lng, lat);
		double[] wgs84 = gcj02towgs84(gcj[0], gcj[1]);
		return wgs84;
	}

	/**
	 * WGS坐标转百度坐标系(BD-09)
	 * 
	 * @param lng WGS84坐标系的经度
	 * @param lat WGS84坐标系的纬度
	 * @return 百度坐标数组
	 */
	public static double[] wgs84tobd09(double lng, double lat) {
		double[] gcj = wgs84togcj02(lng, lat);
		double[] bd09 = gcj02tobd09(gcj[0], gcj[1]);
		return bd09;
	}

	/**
	 * 火星坐标系(GCJ-02)转百度坐标系(BD-09)
	 * 
	 * 谷歌、高德——>百度
	 * @param lng 火星坐标经度
	 * @param lat 火星坐标纬度
	 * @return 百度坐标数组
	 */
	public static double[] gcj02tobd09(double lng, double lat) {
		double z = Math.sqrt(lng * lng + lat * lat) + 0.00002 * Math.sin(lat * x_pi);
		double theta = Math.atan2(lat, lng) + 0.000003 * Math.cos(lng * x_pi);
		double bd_lng = z * Math.cos(theta) + 0.0065;
		double bd_lat = z * Math.sin(theta) + 0.006;
		return new double[] { bd_lng, bd_lat };
	}

	/**
	 * 百度坐标系(BD-09)转火星坐标系(GCJ-02)
	 * 
	 * 百度——>谷歌、高德
	 * @param bd_lon 百度坐标纬度
	 * @param bd_lat 百度坐标经度
	 * @return 火星坐标数组
	 */
	public static double[] bd09togcj02(double bd_lon, double bd_lat) {
		double x = bd_lon - 0.0065;
		double y = bd_lat - 0.006;
		double z = Math.sqrt(x * x + y * y) - 0.00002 * Math.sin(y * x_pi);
		double theta = Math.atan2(y, x) - 0.000003 * Math.cos(x * x_pi);
		double gg_lng = z * Math.cos(theta);
		double gg_lat = z * Math.sin(theta);
		return new double[] { gg_lng, gg_lat };
	}

	/**
	 * WGS84转GCJ02(火星坐标系)
	 * 
	 * @param lng WGS84坐标系的经度
	 * @param lat WGS84坐标系的纬度
	 * @return 火星坐标数组
	 */
	public static double[] wgs84togcj02(double lng, double lat) {
		if (out_of_china(lng, lat)) {
			return new double[] { lng, lat };
		}
		double dlat = transformlat(lng - 105.0, lat - 35.0);
		double dlng = transformlng(lng - 105.0, lat - 35.0);
		double radlat = lat / 180.0 * pi;
		double magic = Math.sin(radlat);
		magic = 1 - ee * magic * magic;
		double sqrtmagic = Math.sqrt(magic);
		dlat = (dlat * 180.0) / ((a * (1 - ee)) / (magic * sqrtmagic) * pi);
		dlng = (dlng * 180.0) / (a / sqrtmagic * Math.cos(radlat) * pi);
		double mglat = lat + dlat;
		double mglng = lng + dlng;
		return new double[] { mglng, mglat };
	}

	/**
	 * GCJ02(火星坐标系)转GPS84
	 * 
	 * @param lng 火星坐标系的经度
	 * @param lat 火星坐标系纬度
	 * @return WGS84坐标数组
	 */
	public static double[] gcj02towgs84(double lng, double lat) {
		if (out_of_china(lng, lat)) {
			return new double[] { lng, lat };
		}
		double dlat = transformlat(lng - 105.0, lat - 35.0);
		double dlng = transformlng(lng - 105.0, lat - 35.0);
		double radlat = lat / 180.0 * pi;
		double magic = Math.sin(radlat);
		magic = 1 - ee * magic * magic;
		double sqrtmagic = Math.sqrt(magic);
		dlat = (dlat * 180.0) / ((a * (1 - ee)) / (magic * sqrtmagic) * pi);
		dlng = (dlng * 180.0) / (a / sqrtmagic * Math.cos(radlat) * pi);
		double mglat = lat + dlat;
		double mglng = lng + dlng;
		return new double[] { lng * 2 - mglng, lat * 2 - mglat };
	}

	/**
	 * 纬度转换
	 * 
	 * @param lng
	 * @param lat
	 * @return
	 */
	public static double transformlat(double lng, double lat) {
		double ret = -100.0 + 2.0 * lng + 3.0 * lat + 0.2 * lat * lat + 0.1 * lng * lat + 0.2 * Math.sqrt(Math.abs(lng));
		ret += (20.0 * Math.sin(6.0 * lng * pi) + 20.0 * Math.sin(2.0 * lng * pi)) * 2.0 / 3.0;
		ret += (20.0 * Math.sin(lat * pi) + 40.0 * Math.sin(lat / 3.0 * pi)) * 2.0 / 3.0;
		ret += (160.0 * Math.sin(lat / 12.0 * pi) + 320 * Math.sin(lat * pi / 30.0)) * 2.0 / 3.0;
		return ret;
	}

	/**
	 * 经度转换
	 * 
	 * @param lng
	 * @param lat
	 * @return
	 */
	public static double transformlng(double lng, double lat) {
		double ret = 300.0 + lng + 2.0 * lat + 0.1 * lng * lng + 0.1 * lng * lat + 0.1 * Math.sqrt(Math.abs(lng));
		ret += (20.0 * Math.sin(6.0 * lng * pi) + 20.0 * Math.sin(2.0 * lng * pi)) * 2.0 / 3.0;
		ret += (20.0 * Math.sin(lng * pi) + 40.0 * Math.sin(lng / 3.0 * pi)) * 2.0 / 3.0;
		ret += (150.0 * Math.sin(lng / 12.0 * pi) + 300.0 * Math.sin(lng / 30.0 * pi)) * 2.0 / 3.0;
		return ret;
	}

	/**
	 * 判断是否在国内，不在国内不做偏移
	 * 
	 * @param lng
	 * @param lat
	 * @return
	 */
	public static boolean out_of_china(double lng, double lat) {
		if (lng < 72.004 || lng > 137.8347) {
			return true;
		} else if (lat < 0.8293 || lat > 55.8271) {
			return true;
		}
		return false;
	}
}
~~~



## 通知

**示例代码**

```java
Notification notification = new NotificationCompat.Builder(this)
                .setContentIntent(PendingIntent.getActivity(this, 0, new Intent(this, MainActivity.class), 0))
                .setSmallIcon(R.mipmap.ic_launcher_round)
                .setContentTitle("测试入口")
                .setContentText("测试环境下可见，更改环境配置的入口")
                .setPriority(NotificationCompat.PRIORITY_MAX)
                .setWhen(System.currentTimeMillis())
                .setDefaults(NotificationCompat.DEFAULT_SOUND)
                .build();
        notification.flags |= Notification.FLAG_NO_CLEAR;
        NotificationManagerCompat.from(this).notify((int) System.currentTimeMillis() >> 10, notification);
```



## MaterialDesign

**水波纹**

android:background="?android:attr/selectableItemBackground"波纹有边界
android:background="?android:attr/selectableItemBackgroundBorderless"波纹超出边界

android:colorControlHighlight：设置波纹颜色
android:colorAccent：设置checkbox等控件的选中颜色