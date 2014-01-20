## 安装完UBUNTU 12.10后你要做的十件事 ##

### 1.更新你的系统 ###

你可以运行Software Updater或直接执行命令：

	sudo apt-get update
	sudo apt-get upgrade

### 2.安装一些必要的程序/媒体解码器 ###

为了安装这些常见的媒体解码器，只安装Ubuntu受限的额外包。打开一个终端，然后执行：

	sudo apt-get install ubuntu-restricted-extras

为了播放加密的DVDs，你还需要安装libdvdcss，执行以下命令:

	sudo apt-get install libdvdread4
	sudo /usr/share/doc/libdvdread4/install-css.sh

为了压缩和解压文件，安装7zip：

	sudo apt-get install p7zip-full p7zip-rar

### 3.给你的设备安装驱动 ###

**安装Webcam软件：**

为了控制摄像头，你可以试试cheese或guvciewer：

	sudo apt-get install cheese

或者：

	sudo add-apt-repository ppa:pj-assis/ppa
	sudo apt-get update
	sudo apt-get install guvcview

**安装图形显卡驱动**

在附加驱动区域，选择你要安装的驱动。

### 4.安装VLC——多合一的多媒体播放器 ###

为了安装vlc只要执行：

	sudo apt-get install vlc

### 查找并安装你喜欢的应用 ###

在Ubuntu Software Center查找并安装你喜欢的应用。

需要图形编辑软件?试试GIMP：

	sudo apt-get install gimp

需要一个像winamp一样的音乐播放器？试试Audacious：

	sudo apt-get install audacious

### 6.禁用Unity Dash search中的Amazon search ###

在Privacy Settings -> Search Results中进行设置。

或者你可以卸载掉unity lens for shopping：

	sudo apt-get remove unity-lens-shopping

### 7.设置备份程序 ###

Dejadup是安装在Ubuntu 12.10的默认的备份程序。

你可以试试Dropbox：

	sudo apt-get install nautilus-dropbox

或Ubuntu One

### 8.不喜欢Unity？试试其它桌面环境 ###

如果你想要安装xfce桌面环境，只要执行：

	sudo apt-get install xfce4

安装KDE桌面环境：

	sudo apt-get install kde-plasma-desktop

### 9.定制和Tweak桌面设置 ###

**Unsettings：定制Unity**

为了安装Unsetting，只要执行：

	sudo apt-add-repository ppa:diesch/testing
	sudo apt-get update
	sudo apt-get install unsettings

**Gnome Tweak Tool：定制Gnome 3 shell**

执行：

	sudo apt-get install gnome-tweak-tool

**安装Ubuntu Tweak**

执行：

	sudo add-apt-repository ppa:tualatrix/ppa
	sudo apt-get update
	sudo apt-get install ubuntu-tweak

### 10.安装Synaptic包管理器 ###

为了安装Synaptic包管理器，你只要执行：

	sudo apt-get install synaptic

链接：[10 THINGS TO DO AFTER INSTALLING UBUNTU 12.10](http://blog.sudobits.com/2012/10/10/10-things-to-do-after-installing-ubuntu-12-10/)