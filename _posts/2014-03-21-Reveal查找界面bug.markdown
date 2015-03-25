---
title: Reveal查看界面信息
date: 2014-12-24 19:55:16
categories: jekyll testing
---

晚上加班看了一下Reveal的教程，参照教程将Reveal玩了一下

需要越狱的机器一台，安装Openssh和Cydia SubStrate（一个减少卡顿和崩溃的插件，非必须）
![Image]({{ site.baseurl }}/images/reveal1.jpg)
![Image]({{ site.baseurl }}/images/reveal2.jpg)

进入/Library/MobileSubstrate/DynamicLibraries/,将Reveal.framework libReveal.dylib libReveal.plist三个文件拷贝到该目录。
其中libReveal.plist为App的bundleIdentifier：

{
	Filter = {
		Bundles = (
			"com.apple.store",	//AppStore
			"com.tencent.xin",	//某信
		);
	};

>cd /Library/MobileSubstrate/DynamicLibraries/
>scp -r ned@192.168.0.105:/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/Reveal.framework .
>scp -r ned@192.168.0.105:/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib .
>scp -r ned@192.168.0.105:/Users/ned/Desktop/libReveal.plist .

##3.重启机器 重启客户端
killall SpringBoard

##4.查看App是否正确被动态链接。
Reveal.framework被加载后，会用mDNS协议在同一局域网发布一个类型为_reveal.tcp.的服务，确保能发现该服务。
![Image]({{ site.baseurl }}/images/reveal4.jpg)

##5.确保服务开启后，打开Reveal，查看界面布局，诸如视图，属性，约束，层次等信息，有助于界面debug.
![Image]({{ site.baseurl }}/images/reveal6.jpg)

