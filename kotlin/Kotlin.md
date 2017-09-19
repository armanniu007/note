
# Kotlin

## 基本类型

## 集合

**读写操作可拆分：** kotlin区分可变集合和不可变集合（lists，sets，maps ...）。

*不可变集合：* list，map，set ...

*可变集合：* MutableList， MutableMap，MutableSet ...

**创建集合**

``` kotlin
listOf(1, 2, 3)
mapOf(a to b, c to d)
setOf(s)	//无序，只能存在一个null，不能存在重复数据
items.filter{it%2==0} // 返回 [2,4]
```

**集合扩展函数**

```kotlin
val items = listOf(1, 2, 3, 4)
items.first() == 1
items.last() == 4 items.filter{it%2==0}
items.filter{it%2==0} // 返回 [2,4]
```



## 扩展

**扩展的作用域：** *扩展函数的声明位置决定了扩展函数的作用域。* 一种是在包内顶级声明，一种是在类内部声明。显然，前一种的声明方式作用域更广泛，事实也是如此，这种声明方式可以被其它包引用，可以被引用包内的函数调用。后一种方式的作用域是类成员可以调用。

包内顶层声明扩展示例代码：

```kotlin
package com.davdian.seller.extensions

import android.content.Context
import android.util.DisplayMetrics
import android.util.TypedValue
import android.view.WindowManager


/**
 * Created by bigniu on 2017/6/17.
 * Context的扩展函数
 */
fun Context.dp2px(dp: Float): Int {
    val px = TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dp, resources.displayMetrics).toInt()
    return px
}
```

类成员声明示例代码：

```kotlin
class MainTabLayout : RelativeLayout {

    constructor(context: Context) : this(context, null)
    constructor(context: Context, attrs: AttributeSet?) : this(context, attrs, 0)
    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr)

    private fun TextView.setTextColor(defColor: Int, selectColor: Int) {
        val colorStateList = ColorStateList(arrayOf(intArrayOf(android.R.attr.state_selected),
                intArrayOf(0)), intArrayOf(selectColor, defColor))
        setTextColor(colorStateList)
    }

}
```

***注意***  扩展函数内的this代表调用者（.前的对象）

## 非空判断

**?** 作为非空判断操作字符，**?:** 代表的意思是前置条件为为空

获取属性**YY**的值：

```kotlin
//java 代码
if(var!=null){
	XX xx = var.getxx();
	if(xx!=null){
		YY yy=xx.getyy();
	}
}

//kotlin 代码
yy:YY=var?.getxx()?.getyy()
```

如果需要给属性**YY**默认值：

```kotlin
//java 代码
YY yy=default;
if(var!=null){
	XX xx = var.getxx();
	if(xx! = null && xx.getyy() != null){
		yy=xx.getyy();
	}
}

//kotlin 代码
yy:YY=var?.getxx()?.getyy()?:default
```

## 范型

**生产者** *out* 限定上界，如``List<out Class<Fragment>>``

**消费者** *in* 限定下界，如:

```kotlin
fun sort(to:List<String>,compator<in String>:Compator){
	
}
```



## 类和对象

