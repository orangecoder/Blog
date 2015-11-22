title: 解决Mac下无法识别Android手机的问题
date: 2015-10-15 00:26:38
tags:
---
### 1. 插入手机，打开命令行终端，输入以下命令
```{bash}
	system_profiler SPUSBDataType
```
得到以下结果
![](/image/解决Mac下无法识别Android手机的问题-1.png)

### 2. 如果没有得到上面 Vendor ID ，拔出手机，重复以上动作

### 3. 在命令行终端输入下面命令 （0x2a45 根据你的 Vendor ID 值来替换）
```{bash}
	echo "0x2a45" > ~/.android/adb_usb.ini
```

### 4. 重启adb后就可以了
```{bash}
	adb kill-server
```