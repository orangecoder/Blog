title: mac下iterm2和zsh配置
date: 2016-01-28 11:01:30
tags:
---
首先看张最终的效果图
![](/image/iterm2-zsh-1.png)

### 1.下载安装iterm2
* 下载安装iterm2 [下载链接](http://www.iterm2.com)
* 将bash切换到zsh
```{bash}
chsh -s /bin/zsh
```

### 2.下载安装oh-my-zsh
```{bash}
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

### 3.下载安装Powerline
powerline是一款非常好用的代码提示状态栏，[官网地址](http://powerline.readthedocs.org/en/latest/index.html)
如果你的终端能够正常执行pip指令，那么直接执行下面的指令可以完成安装
```{bash}
pip install powerline-status
```
如果没有，则先执行安装pip指令
```{bash}
sudo easy_install pip
```

### 4.设置iterm2的Regular Font 和 Non-ASCII Font
* 下载powerline字体库 
```{bash}
git clone https://github.com/powerline/fonts.git
```
* cd到install.sh文件所在目录,执行./install.sh指令安装所有Powerline字体
* 把iTerm 2的设置里的Profile中的Text 选项卡中里的Regular Font和Non-ASCII Font的字体都设置成 Powerline的字体，我这里设置的字体是12pt Meslo LG S DZ Regular for Powerline
![](image/iterm2-zsh-2.png)

### 5.设置iterm2的配色方案
* 下载solarized 
```{bash}
git clone https://github.com/altercation/solarized.git
```
* 进入刚刚下载的工程的solarized/iterm2-colors-solarized 下双击 Solarized Dark.itermcolors 和 Solarized Light.itermcolors 两个文件就可以把配置文件导入到 iTerm2 里
* iterm2配置刚安装的配色主题
![](image/iterm2-zsh-3.png)

### 6.设置iterm2的主题为agnoster
* 下载agnoster 
```{bash}
git clone https://github.com/fcamblor/oh-my-zsh-agnoster-fcamblor.git
```
* 进入刚刚下载的工程里面运行install文件,主题将安装到~/.oh-my-zsh/themes目录下
* 打开~/.zshrc文件，然后将ZSH_THEME后面的字段改为agnoster。ZSH_THEME="agnoster"（agnoster即为要设置的主题）

### 7.增加指令高亮效果
指令高亮效果作用是当用户输入正确命令时指令会绿色高亮，错误时命令红色高亮
* 切换到.zshrc所在目录
```{bash}
cd ~
```
* 下载zsh-syntax-highlighting 
```{bash}
git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
```
* 打开.zshrc文件，在最后添加下面内容
```{bash}
source ~/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
plugins=(zsh-syntax-highlighting)
```

