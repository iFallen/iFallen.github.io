---
layout: post
title: 通过Runtime，修改UIStatusBar文字及背景颜色 
excerpt: "通过Runtime，修改UIStatusBar文字及背景颜色"
tags: [Runtime,iOS]
categories: [iOS]

  
comments: true

---


<!--image:
  //不需要img路径
  feature: Gallary/716.jpg 
  feature: http://21232
  credit: JuanFelix
  creditlink: -->
  
[版权所有,转载请注明出处!](https://ifallen.github.io)


### `通过Runtime，修改UIStatusBar文字及背景颜色`

---

#### `1、白色`

>
如果只是单纯的将状态栏修改为白色就简单了。

- `iOS9.0 之前`

```
1、代码设置：
[[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
2、Info.plist 添加：
View controller-based status bar appearance = NO
```

- `iOS9.0 之后`

>
在第一层Controller添加如下代码（不需要修改Info.plist）
>
注意，如果用的是NavigationController，那么在NavigationController里面设置，不是在rootViewController中设置。


```
-(UIStatusBarStyle)preferredStatusBarStyle{
    return UIStatusBarStyleLightContent;
}
```

#### `2、自定义颜色`

>
白色估计大家都改过，针对自定义颜色的修改我们需要借助`Runtime`来实现（不要试图通过UIStatusBar实例化一个，没有提供这种方法，就算实例化一个对象也不是系统的那个）。
>
首先，我们要知道StatusBar是被Application持用的。所以我们的先通过Application 找到我们的StatusBar对象.

- `1、导入头文件`

```
#import <objc/runtime.h>
```

- `2、获取类的某个变量描述`

```
Ivar istb =  class_getInstanceVariable([UIApplication class], "_statusBar");
```

- `3、通过变量描述，获取对象的实例变量，即我们需要的StatusBar`

```
UIApplication * app = [UIApplication sharedApplication];
id statusBar = object_getIvar(app, istb);//我们需要的StatusBar
```

- `4、修改字体颜色和背景颜色`

```
[statusBar performSelector:@selector(setForegroundColor:) withObject:[UIColor orangeColor]];//前景色
[statusBar performSelector:@selector(setBackgroundColor:) withObject:[UIColor colorWithRed:0.3 green:0.8 blue:0.5 alpha:0.35]];//背景色
```
- `5、实际效果`

|效果图 |
|:-----------: |
|![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/statusbar/statusbar.png)|


#### `3、关于Runtime说两句`

>
上面我们只是修改了颜色，其实还有很多属性可以修改。这里也只用到了Runtime的一点点知识,Runtime如果只用在这里也就大材小用了。如果想要更过的了解，可以花一周的时间把Runtime的知识从头到尾理一道。
>
可以从[这篇博客](http://southpeak.github.io/blog/2014/10/25/objective-c-runtime-yun-xing-shi-zhi-lei-yu-dui-xiang/)入门。

>
面试官经常拿Runtime、RunLoop来说事，pass了好多学生,哎...
>
一个`UIKit`+`AF`+`Cocoapods/Carthage`+`Git`+`MVC`+`支付分享`，初、中级程序员把这些`用好!用好!`,估计已经可以应对N多中小项目了。大家总是拿什么百万、千万级的项目来说事，还没到十万就已经死了。
>
当然,了解总归是好的。技术路还很长，共勉！
>
`用好当下技术，解决好当下问题！`
>
`奈何只愿做个程序猿渣渣`


---

	
