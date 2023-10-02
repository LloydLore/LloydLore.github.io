+++
title = "doomemacs GUI 中文显示及输入法设置"
author = ["丛朝"]
date = 2023-10-03T00:00:00+08:00
lastmod = 2023-10-03T01:18:00+08:00
tags = ["hugo", "emacs", "org"]
categories = ["emacs"]
draft = false
weight = 2001
toc = true
isCJKLanguage = true
+++

本文尝试对 doomemacs 的中文字体显示及输入法设置，做一个记录。先说我的配置：

    中文显示字体，[Sarasa Gothic SC](https://github.com/be5invis/Sarasa-Gothic)
    中文输入法，[PYIM](https://github.com/tumashu/pyim) + [liberime](https://github.com/merrickluo/liberime)
    输入法方案，[四叶草](https://www.fkxxyz.com/d/cloverpinyin/)
    其他…


## doomemacs GUI frame 中文显示 {#doomemacs-gui-frame-中文显示}

我一般只会使用窗口模式下的 emacs，此前在使用我自身做的 emacs 配置时，发现配置中文只要简单地设置 CJK 字符就可以，但到了用 doomemacs 时，我尝试了各种方法，总是不行。[最终发现了一个 hook 的使用](https://emacs-china.org/t/doom-emacs/23513/10)，如下：

```emacs-lisp
(defun +set-cjk()
  "set cjk for Emacs"
  (interactive)
  (dolist (charset '(kana han symbol cjk-misc bopomofo))
    (set-fontset-font (frame-parameter nil 'font)
                      charset
                      (font-spec :family "Sarasa Gothic SC"))))

(add-hook 'after-setting-font-hook #'+set-cjk)
```

即在 doomemacs 设置完字体以后，我们再来覆盖 CJK 的字体。


## 中文输入法，PYIM 和 liberime {#中文输入法-pyim-和-liberime}

在 emacs 下，PYIM, emacs-rime, [SIS](https://github.com/laishulu/emacs-smart-input-source)，我都用过，我在这里不赘述几种输入法的优劣。单从我个 人的使用习惯来说，我更喜欢 PYIM + liberime 的组合，就像 PYIM 作者说的那样，PYIM 就像我的朋友一样，一直陪伴的着我，而我唯一觉得用得不舒服的地方，在于 PYIM 自带字库，以及云输入法，我总觉得会有些延迟。

基于以上的考虑，最终我想用 PYIM 作为前端，rime 输入法引擎作为后端。至少试下来，整体的效果还是很好的。


## Rime 输入法方案——四叶草 {#rime-输入法方案-四叶草}

我喜欢这个输入法方案。我的个人电脑上，安装的是 Arco Linux, 有些字体的支持，emoji 的支持有限，这里着重说下几个问题。


### 问题一，输入法 emoji 不显示 {#问题一-输入法-emoji-不显示}

安装 fontconfig 的配置，见 <https://github.com/szclsya/dotfiles/blob/master/fontconfig/fonts.conf>


### 问题二，四叶草输入方案 {#问题二-四叶草输入方案}

最好不要和系统输入法共用一个。
