title: SurfaceView, GLSurfaceView, SurfaceTexture 和 TextureView 总结
date: 2016-05-13 13:28:55
tags:
---
* SurfaceView

从 Android 1.0(API level 1)时就有 。它继承自类 View，因此它本质上是一个 View。但与普通 View 不同的是，它有自己的 Surface。我们知道，一般的 Activity 包含的多个 View 会组成 View hierachy 的树形结构，只有最顶层的 DecorView，也就是根结点视图，才是对 WMS 可见的。这个 DecorView 在 WMS 中有一个对应的 WindowState。相应地，在 SF 中对应的 Layer。而 SurfaceView 自带一个 Surface，这个 Surface 在 WMS 中有自己对应的 WindowState，在 SF 中也会有自己的 Layer。如下图所示：
![](/image/android-views-1.png)
也就是说，虽然在 App 端它仍在 View hierachy 中，但在 Server 端（WMS和SF）中，它与宿主窗口是分离的。这样的好处是对这个 Surface 的渲染可以放到单独线程去做，渲染时可以有自己的 GL context。这对于一些游戏、视频等性能相关的应用非常有益，因为它不会影响主线程对事件的响应。但它也有缺点，因为这个 Surface 不在 View hierachy 中，它的显示也不受 View 的属性控制，所以不能进行平移，缩放等变换，也不能放在其它 ViewGroup 中，一些 View 中的特性也无法使用。

* GLSurfaceView

从 Android 1.5(API level 3) 开始加入，作为 SurfaceView 的补充。它可以看作是 SurfaceView 的一种典型使用模式。在 SurfaceView 的基础上，它加入了 EGL 的管理，并自带了渲染线程。另外它定义了用户需要实现的 Render 接口，提供了用 Strategy pattern 更改具体 Render 行为的灵活性。作为 GLSurfaceView 的 Client，只需要将实现了渲染函数的 Renderer 的实现类设置给 GLSurfaceView 即可。相关类图如下。其中 SurfaceView 中的 SurfaceHolder 主要是提供了一坨操作 Surface 的接口。GLSurfaceView 中的 EglHelper 和 GLThread 分别实现了上面提到的管理 EGL 环境和渲染线程的工作。GLSurfaceView 的使用者需要实现 Renderer 接口。
![](/image/android-views-2.png)

* SurfaceTexture

从 Android 3.0(API level 11)加入。和 SurfaceView 不同的是，它对图像流的处理并不直接显示，而是转为 GL 外部纹理，因此可用于图像流数据的二次处理（如 Camera 滤镜，桌面特效等）。比如 Camera 的预览数据，变成纹理后可以交给 GLSurfaceView 直接显示，也可以通过 SurfaceTexture 交给 TextureView 作为 View heirachy 中的一个硬件加速层来显示。首先，SurfaceTexture 从图像流（来自 Camera 预览，视频解码，GL 绘制场景等）中获得帧数据，当调用 updateTexImage() 时，根据内容流中最近的图像更新 SurfaceTexture 对应的 GL 纹理对象，接下来，就可以像操作普通 GL 纹理一样操作它了。从下面的类图中可以看出，它核心管理着一个 BufferQueue 的 Consumer 和 Producer 两端。Producer 端用于内容流的源输出数据，Consumer 端用于拿 GraphicBuffer 并生成纹理。SurfaceTexture.OnFrameAvailableListener 用于让 SurfaceTexture 的使用者知道有新数据到来。JNISurfaceTextureContext 是 OnFrameAvailableListener 从 Native 到 Java 的 JNI 跳板。其中 SurfaceTexture 中的 attachToGLContext() 和 detachToGLContext() 可以让多个 GL context 共享同一个内容源。
![](/image/android-views-3.png)
Android 5.0 中将 BufferQueue 的核心功能分离出来，放在 BufferQueueCore 这个类中。BufferQueueProducer 和 BufferQueueConsumer 分别是它的生产者和消费者实现基类（分别实现了 IGraphicBufferProducer 和 IGraphicBufferConsumer 接口）。它们都是由 BufferQueue 的静态函数 createBufferQueue() 来创建的。Surface 是生产者端的实现类，提供 dequeueBuffer/queueBuffer 等硬件渲染接口，和 lockCanvas/unlockCanvasAndPost 等软件渲染接口，使内容流的源可以往 BufferQueue 中填 graphic buffer。GLConsumer 继承自 ConsumerBase，是消费者端的实现类。它在基类的基础上添加了 GL 相关的操作，如将 graphic buffer 中的内容转为 GL 纹理等操作。到此，以 SurfaceTexture 为中心的一个 pipeline 大体是这样的：
![](/image/android-views-4.png)

* TextureView

在 4.0(API level 14) 中引入。它可以将内容流直接投影到 View 中，可以用于实现 Live preview 等功能。和 SurfaceView 不同，它不会在 WMS 中单独创建窗口，而是作为 View hierachy 中的一个普通 View，因此可以和其它普通 View 一样进行移动，旋转，缩放，动画等变化。值得注意的是 TextureView 必须在硬件加速的窗口中。它显示的内容流数据可以来自 App 进程或是远端进程。从类图中可以看到，TextureView 继承自 View，它与其它的 View 一样在 View hierachy 中管理与绘制。TextureView 重载了 draw() 方法，其中主要把 SurfaceTexture 中收到的图像数据作为纹理更新到对应的 HardwareLayer 中。SurfaceTexture.OnFrameAvailableListener 用于通知 TextureView 内容流有新图像到来。SurfaceTextureListener 接口用于让 TextureView 的使用者知道 SurfaceTexture 已准备好，这样就可以把 SurfaceTexture 交给相应的内容源。Surface 为 BufferQueue 的 Producer 接口实现类，使生产者可以通过它的软件或硬件渲染接口为 SurfaceTexture 内部的 BufferQueue 提供 graphic buffer。
![](/image/android-views-5.jpg)

最后，总结下这几者的区别和联系。简单地说，SurfaceView 是一个有自己 Surface 的 View。它的渲染可以放在单独线程而不是主线程中。其缺点是不能做变形和动画。SurfaceTexture 可以用作非直接输出的内容流，这样就提供二次处理的机会。与 SurfaceView 直接输出相比，这样会有若干帧的延迟。同时，由于它本身管理 BufferQueue，因此内存消耗也会稍微大一些。TextureView 是一个可以把内容流作为外部纹理输出在上面的 View。它本身需要是一个硬件加速层。事实上 TextureView 本身也包含了 SurfaceTexture。它与 SurfaceView+SurfaceTexture 组合相比可以完成类似的功能（即把内容流上的图像转成纹理，然后输出）。区别在于 TextureView 是在 View hierachy 中做绘制，因此一般它是在主线程上做的（在Android 5.0引入渲染线程后，它是在渲染线程中做的）。而 SurfaceView+SurfaceTexture 在单独的 Surface 上做绘制，可以是用户提供的线程，而不是系统的主线程或是渲染线程。另外，与 TextureView 相比，它还有个好处是可以用 Hardware overlay 进行显示。

[参考](http://blog.csdn.net/huqingwang/article/details/45394217)


