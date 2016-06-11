title: Android 开发总结
date: 2015-10-22 23:14:32
tags:
---
### 1. [Android 中的 Service 全面总结](http://www.cnblogs.com/newcj/archive/2011/05/30/2061370.html)
* Service 的种类
* Service 与 Thread 的区别
* Service 的生命周期
* startService 启动服务
* Local 与 Remote 服务绑定
* 创建前台服务
* 在什么情况下使用 startService 或 bindService 或 同时使用startService 和 bindService
* 在 AndroidManifest.xml 里 Service 元素的常见选项

### 2. [Android中BroadCastReceiver使用](http://www.cnblogs.com/jico/articles/1838293.html)
* 静态注册
* 动态注册

### 3. [Android中的几种多线程实现 ](http://blog.sina.com.cn/s/blog_74e9d98d0101g9iw.html)
* Activity.runOnUiThread(Runnable)
* View.post(Runnable)
* Handler, HandlerThread 
* AsyncTask, LoaderManager

### 4. [View 事件传递](http://a.codekk.com/detail/Android/Trinea/公共技术点之%20View%20事件传递)
* Touch 事件都被封装成了 MotionEvent 对象
* 事件类型分为 ACTION_DOWN, ACTION_UP, ACTION_MOVE, ACTION_POINTER_DOWN, ACTION_POINTER_UP, ACTION_CANCEL
* 对事件的处理包括三类，分别为传递——dispatchTouchEvent()函数、拦截——onInterceptTouchEvent()函数、消费——onTouchEvent()函数和 OnTouchListener

### 5. [View 绘制流程](http://a.codekk.com/detail/Android/lightSky/公共技术点之%20View%20绘制流程)
* measure
* layout
* draw
* invalidate(), requestLayout() 

### 6. [Android 动画基础](http://a.codekk.com/detail/Android/lightSky/公共技术点之%20Android%20动画基础)
* 传统 View 动画: Tween(alpha,scale,translate,rotate), Frame
* Property Animation: ValueAnimator, ObjectAnimator

### 7. [Android SQlite 总结](http://blog.csdn.net/liuhaomatou/article/details/23797107)
* Android平台下数据库相关类: SQLiteOpenHelper, SQLiteDatabase, SQLiteCursor
* SQLite内建语法表
* SQLite内建函数表

### 8. [Android 布局优化](http://www.infoq.com/cn/articles/android-optimise-layout)
* 尽量多使用 RelativeLayout，不要使用绝对布局 AbsoluteLayout
* 将可复用的组件抽取出来并通过 `<include/>` 标签使用
* 使用 `<ViewStub/>` 标签来加载一些不常用的布局
* 使用 `<merge/>` 标签减少布局的嵌套层次

