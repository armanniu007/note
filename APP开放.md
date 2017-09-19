# 账户服务

**账户服务** 向APP提供所有的账户数据，以及账户行为操作，通过AccountManager进行管理。

所有的数据都包含在UserModel中，通过AccountManager获取UserModel，你不能设置一个UserModel。

通过AccountManager可以进行登录、注册、修改、退出等账户操作行为。需要注意的是，退出登录相当于登录了一个特殊的账户，默认为游客。

当账户的属性发生改变的时候，***服务*** 会向外发送广播事件，该广播面向所有的注册者，如果你只关心某些属性的改变，你需要对事件进行判断。



## 网络请求中的sessionKey和shopUrl的获取和插入



[TOC]





## UserModel

**属性**





**method**

\- setSessionKeyAndShopUrlIngoreChanged() : void

\+ set..

\+ get..



## AccountDataEvent

###Summary





## LoginActivity

### method

1. toLogin() : void	//打开登录界面
2. toRegist(): void
3. toResetPwd():void
4. toAddInvite():void
5. toOpenShop(): void
6. login()
7. regist()
8. resetPwd()
9. addInvite()
10. openShop()
11. onGuideEnd()









