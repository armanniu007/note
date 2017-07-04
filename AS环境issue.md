# AS环境issue
> 这里记录了在使用android studio编译代码中遇到的一些问题
> 
> 1. 远程依赖重复引用的问题
> 2. adb 发送过薄页记录到了这里 

## 1. AS重复依赖处理

1. 发现问题

	```
	Error:Execution failed for task ':app:transformClassesWithDexForDebugAndroidTest'.> com.android.build.api.transform.TransformException: com.android.ide.common.process.ProcessException: java.util.concurrent.ExecutionException: com.android.dex.DexException: Multiple dex files define Lorg/hamcrest/Description;
	```
	
	日志中写到（org/hamcrest/Description）发生冲突
	
2. 打开目录（.idea/libraries）


3. 找到"hamcrest\_core\_1\_3"文件


4. 排除法找到倒入"hamcrest\_core\_1\_3"的库为：  
	
	"com.android.support.test.espresso:espresso-core:2.2.2"
	
5. 依赖中添加代码
  
	```
	androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
		exclude group: 'com.android.support', module: 'support-annotations'
		exclude module: 'hamcrest-core'
	})
	```
	
## 2.	AS导入／创建Module失败
**表现：**左侧gradle列表中对应module变灰色，project structure中modules列表没有新建的module

**原因：**未知

**解决：**点击【app】标签，选择【module】，右键【module】选择configure project subset，勾选对应的选项将【module】关联到工程目录



## 3. adb发送广播
在做推送的时候，项目中特殊需求,根据以下三个维度处理业务逻辑

1. 推送的数据
2. 触发的时机（收到推送或点击通知栏）
3. app状态（前台、后台、未启动）

客户端完成功能后和后台联调，由于客户端的bug严重拖延了联调进度（不要问我为什么没有自测-_-!）。

看我的栗子：

```
adb shell am broadcast -a 'com.davdian.debug' -e 'cmd'  'davdian://call.Community.com?action=openCommunity\&params=\&callback=\&minv=2.4.0'
```
***-e*** `[-e|--es <EXTRA_KEY> <EXTRA_STRING_VALUE> ...]` 添加String类型的数据。

其它参数使用方法可以通过`adb help am broadcast`命令查看，有困难不要忘记神器*<color=blue>Google</color>*

**<a>注意：</a>** *&*符号在终端是连接多个命令的特殊符号，作为字符串使用需要转义
