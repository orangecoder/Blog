title: ReactNative 开发工具 Atom 配置
date: 2016-03-18 09:38:00
tags:
---

[ReactNative](http://facebook.github.io/react-native/) 是 Facebook 推出的基于 JavaScript 的开源框架。React Native 结合了 Web 应用和 Native 应用的优势，可以使用 JavaScript 来开发 iOS 和 Android 原生应用。在 JavaScript 中用 React 抽象操作系统原生的 UI 组件，代替 DOM 元素来渲染等。

[Atom](https://atom.io) 是 Github 推出的一款号称“属于21世纪”的代码编辑器, 其最大的特点是使用 node.js 来作为其插件语言，以下介绍一下用 Atom 开发 ReactNative 用到的比较好的插件。

### [Nuclide](http://nuclide.io)
Nuclide 是 Facebook 推出的 Atom 插件，作为基于文档编辑器 Atom 的软件包库，Nuclide提供了类似IDE的功能，主要用于简化原生移动应用的开发。

安装完 Atom 后，打开 Settings 面板，并点击 Install 选项卡，然后在搜索框中键入 nuclide ，如图所示：
![](/image/react-native-devtool-1.png)
点击该插件旁边的蓝色 Install 按钮进行安装，安装完 Nuclide 后安装推荐的设置如图：
![](/image/react-native-devtool-2.png)

下面介绍的插件的安装方式都是跟这一样的
### [save-session](https://atom.io/packages/save-session)
让 Atom 记住上一次打开的会话

### [hyperclick](https://atom.io/packages/hyperclick) 和 [js-hyperclick](https://atom.io/packages/js-hyperclick)
跳转对于调试代码和阅读代码非常重要, 安装 hyperclick 和 js-hyperclick , 就可以通过引用跳转到需要类和方法

### [docblockr](https://atom.io/packages/docblockr)
代码注释插件

### [atom-typescript](https://atom.io/packages/atom-typescript)
类型显示