+++
title = "before-save-hook 默认不是 buffer local variable"
author = ["丛朝"]
date = 2024-01-15T00:00:00+08:00
lastmod = 2024-01-15T15:53:00+08:00
tags = ["org", "emacs", "hugo"]
categories = ["emacs"]
draft = false
weight = 2002
toc = true
isCJKLanguage = true
+++

我最近准备用 [reStructuredText](https://docutils.sourceforge.io/rst.html) 做结构化文档的处理。
基于我已经是一个重度的 `org-mode` 患者，我在做任何笔记的时候，离开 `org-mode` 便显得各种
不适应。虽然能看到 [Markdown](https://daringfireball.net/projects/markdown/) 很清秀，reStructuredText 很 sexy，但我大有“野花不如家花香”的
感慨。

每当我吸完其他的工具之后，最后还是乖乖回到了 `org-mode`. 但我不得不承认的是， reStructuredText 配合 [Sphinx](https://www.sphinx-doc.org/en/master/) 有一种化繁为简的美感。我曾经有过想用 `org-mode` 在大型
项目里实践 [Literate programming](https://en.wikipedia.org/wiki/Literate_programming) 的想法，但一想到要如何推广 `org-mode`,整件事情到那儿就
戛然而止了。毕竟推广 `org-mode` 和推广 `GNU Emacs` 一样艰难，人各有喜好，各有接受程度，
`org-mode` 和 `GNU Emacs` 更像是现代武林的邪派武功。

言归正传，我用 `org-mode` 记录了一篇笔记之后，使用 [ox-rst](https://github.com/msnoigrs/ox-rst) 导出到 rst 格式。这里遇到一个问题：
我不想每次保存完 `org-mode` buffer 之后，在手动去做一些导出，那样显得我太蠢了，无法体现出
`GNU Emacs` 的邪典艺术。所以做一个自动导出的设置吧， **Let's trial and error!!!**


## Trial 1 - `after-save-hook` {#trial-1-after-save-hook}

我先想到的是用 `hook`, 一通 `C-h v` 操作下来，试下 `after-save-hook`,代码很简单：

```emacs-lisp
(add-to-hook 'after-save-hook #'org-rst-export-to-rst)
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 1:</span>
  example for bad setting with after-save-hook
</div>

我很悲剧的发现，加了这个 hook 之后，我没有办法保存文件。似乎每次执行导出的操作后，buffer 就变成
编辑过后的状态，我神奇地搞了一个死循环。一直 `save-export-save-export...`.


## Trial 2 - `before-save-hook` {#trial-2-before-save-hook}

既然 `save-buffer` 之后不行，那么就在保存之前吧，毕竟这样很合理。

```emacs-lisp
(add-hook 'before-save-hook #'org-rst-export-to-rst)
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 2:</span>
  example for bad setting with before-save-hook
</div>

OK, 保存的问题解决了。但悲剧总是从一种形式转变成另外一种形式，我以为 `before-save-hook` 是 `local variable`, 但他不是的。我保存的每一个文件，都来了一个 `.rst` 后缀，你能想象 `foo-bar.rst.rst` 的美吗？


## Tiral 3 - `before-save-hook` localization {#tiral-3-before-save-hook-localization}

那…… 有没有一种办法，创造一个 `local variable` 呢？

有的。在[这里](https://stackoverflow.com/questions/1931784/emacs-is-before-save-hook-a-local-variable)我找到了答案。这个答案已经在万维网中存在了 14 年，在今天终于被我捕获了。
进而我看了 `add-hook` 的 manual, 它的 signature 是这样的：

```emacs-lisp
(add-hook HOOK FUNCTION &optional DEPTH LOCAL)
```
<div class="src-block-caption">
  <span class="src-block-number">Code Snippet 3:</span>
  example for good setting with before-save-hook
</div>

就是这个 `LOCAL`, 将它设置成 `t` 之后，会创建一个 `before-save-hook` 的同名 `local variable`. 而这个 hook 的执行顺序，则是先按照 `global variable` 的变量执行一次，
在执行一次 `local variable`.

于是乎，这个问题解决了。额，你说问题是什么？那就是我可以保存完 `org-mode` 的文件后，自动得到
`*.rst` 的文件了。 **Bravo!**


## 更多设置 {#更多设置}

在这里其实还涉及到我对某个 `org-mode buffer` 做的 `local variable` 的设置，举个例子吧：

> `# eval: (add-hook 'before-save-hook #'org-rst-export-to-rst nil t)`

当然，还得加上导出文件的设置，比如：

> `#+export_file_name: /you/export/file/path`

OK, 打完，收功！
