去除Fresco影响的功能点：

1. 音频播放界面，头图显示
2. LiveInfoActivity
3. 首页引导页
4. 音频浮窗，左侧播放按钮背景展示
5. BdGoods6FeedItem
6. BdGoods7FeedItem
7. 课堂搜索
8. 发布发现中的关联商品
9. 视频直播结束
10. 视频直播搜索
11. 课程编辑页面，图片相关操作和展示
12. 视频直播我的关注列表
13. 视频直播、播放，主播头像展示
14. 订单支付，支付成功、取消
15. BdTsGoods0FeedItem
16. BdGoods5FeedItem
17. 课堂笔记，选择图片
18. 个人课程主页
19. 视频个人主页
20. 视频直播关注面板
21. 视频直播，发送红包
22. 视频直播，话题列表（DVDZBTopicListActivity）
23. 视频直播，推荐商品
24. 我的收藏，商品
25. 群聊，群成员列表
26. 老课堂，课堂信息，妈妈消息
27. 推送列表
28. BdStudy0FeedItem
29. 课堂直播，大图预览















```
	<!--头像，尺寸28dp-->
	<style name="Fresco_NormalHead">
    <item name="android:layout_width">@dimen/pxico_head_normal</item>
    <item name="android:layout_height">@dimen/pxico_head_normal</item>
    <item name="placeholderImage">@drawable/ic_head_default</item>
    <item name="failureImage">@drawable/ic_head_default</item>
    <item name="roundingBorderWidth">1dp</item>
    <item name="roundingBorderColor">#FFEEEEEE</item>
    <item name="roundAsCircle">true</item>
```