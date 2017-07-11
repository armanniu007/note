#Android 技术点

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



