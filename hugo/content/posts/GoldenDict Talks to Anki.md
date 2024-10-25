+++
title = "Golden Dict Talks to Anki"
author = ["丛朝"]
date = 2024-10-25T00:00:00+08:00
lastmod = 2024-10-25T16:35:00+08:00
tags = ["Cliche"]
categories = ["只言片语"]
draft = false
toc = true
isCJKLanguage = true
+++

我习惯在电脑上使用离线词典，一来离线词典很快，二来在没有网络的时候，离线词典仍然
能给我一个比较好的结果。其实还有个可能是谬误的想法，我总觉得在线词典，更加吃电脑
的资源，而资源嘛，能省则省啦。

使用过很多词典，对我来说，以及对我儿子来说，欧路词典能够解决大部分平常遇到的问题，
但我总觉在从词典生查的信息，到最终落到内化的知识，欧路词典对于我个人来说，是不太
够的。所以我开始打 `GoldenDict` 的主意了。

最早的 `GoldenDict` 已经很长时间没有维护了，感谢开源作者们重新 fork 出了一个
[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)，这个版本具有检索快，支持大词典，以及快速搜索等强大使用的功能，但
这次我要使用的是一个船新的功能 —— **Anki Integration**.

长久以来，我使用 Anki 的一个比较痛苦的方面就是做卡片。而 GoldenDict-ng 具备一键
制卡的功能，虽然初始的卡片没有那么好看，但也可以通过定制 Anki 卡片的 css 那些方
式来让卡片好看起来。整体来说，这是一个非常实用的功能了。

具体的操作可以分为几步：

1.  安装 Anki Connector (这一步是必须的，无论是使用 Emacs, Chrome Extension 制卡，
    这都需要安装)
2.  设置 GoldenDict-ng 选项
3.  点一下，生成卡片


## 设置 GoldenDict-ng 选项 {#设置-goldendict-ng-选项}

在对 `GoldenDict-ng` 做设置时，我们需要在 `Preferences->Network->Anki Connect`
中设置一些参数，具体可以参考以下：

{{< figure src="/ox-hugo/2024-10-25_16-05-48_screenshot.png" >}}


## 点一下 {#点一下}

设置好 Anki Connect 后，在词典的界面，会多出一个加号，如下图，状态栏会有提示：
`anki:post to anki success`, 这样就可以去 Anki 中查看卡了。

{{< figure src="/ox-hugo/2024-10-25_16-08-48_screenshot.png" >}}


## 小结 {#小结}

希望这条路打通之后，我能多记一些单词吧。Happy Hacking!
