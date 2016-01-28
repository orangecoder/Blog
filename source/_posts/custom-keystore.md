title: android 自定义签名
date: 2016-01-28 13:25:16
tags:
---
生成开发版和发布版的签名，两个签名文件MD5和SHA1值相同，方便第三方插件（微信登录，新浪微博分享，高德地图等等）标识开发者身份
最终效果图
![](/image/custom-keystore-i.png)

### 1.生成正式版的keystore
在app目录下新建keystore文件夹，点击Build->Generate Signed APK... 生成一个正式版的keystore(my.jks)
![](/image/custom-keystore-1.png)

### 2.生成开发版keystore
复制粘贴my.jks,改名为my-debug.jks

### 3.修改my-debug.jks的keystore密码
打开终端，cd到keystore文件夹下执行以下命令：keytool -storepasswd -keystore my-debug.jks
执行后会提示输入证书的当前密码，和新密码以及重复新密码确认。这一步需要将密码改为android
![](/image/custom-keystore-2.png)

### 4.修改keystore的alias
输入命令：keytool -changealias -keystore my-debug.jks -alias orangecoder -destalias androiddebugkey
这个命令会先后提示输入keystore的密码和当前alias的密码。这一步需要将别名修改为androiddebugkey。
![](/image/custom-keystore-3.png)

### 5.修改alias的密码
输入命令：keytool -keypasswd -keystore my-debug.jks -alias androiddebugkey
执行后会提示输入keystore密码，alias密码，然后提示输入新的alias密码，将新密码修改为android
![](/image/custom-keystore-4.png)

### 6.项目签名文件配置
打开app目录下的build.gradle文件，在android任务下加上签名文件设置
![](/image/custom-keystore-5.png)

### 7.看看最终效果
打开gradle任务面板，双击android目录下的signingReport任务，可以看到debug版和release版的MD5和SHA1值都是一样的
![](/image/custom-keystore-6.png)