title: mvc mvp mvvm 总结
date: 2016-06-11 20:06:56
tags:
---
常用的软件架构模式大概可以分为以下三种:
MVC：Model-View-Controller
MVP：Model-View-Presenter
MVVM：Model-View-ViewModel

* 三者的共同点，也就是 Model 和 View
1. Model 就是领域模型，数据对象，同时，提供外部对应用程序数据的操作的接口，也可能在数据变化时发出变更通知。Model 不依赖于 View 的实现，只要外部程序调用 Model 的接口就能够实现对数据的增删改查。
2. View 就是 UI 层，提供对最终用户的交互操作功能，包括 UI 展现代码及一些相关的界面逻辑代码。

* 三者的差异在于如何粘合 View 和 Model，实现用户的交互操作以及变更通知

![](/image/mvc-mvp-mvvm-1.png)

1. Controller 接收 View 的操作事件，根据事件不同，或者调用 Model 的接口进行数据操作，或者进行 View 的跳转，从而也意味着一个 Controller 可以对应多个 View。Controller 对 View 的实现不太关心，只会被动地接收，Model 的数据变更不通过 Controller 直接通知 View，通常 View 采用观察者模式监听 Model 的变化。
2. Presenter，与 Controller 一样，接收 View 的命令，对 Model 进行操作；与 Controller 不同的是 Presenter 会反作用于 View，Model 的变更通知首先被 Presenter 获得，然后 Presenter 再去更新 View。一个 Presenter 只对应于一个 View。根据 Presenter 和 View 对逻辑代码分担的程度不同，这种模式又有两种情况：Passive View 和 Supervisor Controller。
3. ViewModel，注意这里的 “Model” 指的是 View 的 Model，跟上面那个 Model 不是一回事。所谓 View 的 Model 就是包含 View 的一些数据属性和操作的这么一个东东，这种模式的关键技术就是数据绑定（data binding），View 的变化会直接影响 ViewModel，ViewModel 的变化或者内容也会直接体现在 View 上。这种模式实际上是框架替应用开发者做了一些工作，开发者只需要较少的代码就能实现比较复杂的交互。
