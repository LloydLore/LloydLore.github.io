+++
title = "Delete Duplicated Lines in Emacs"
date = 2026-11-02
tags = ["emacs"]
draft = false
weight = 2001
+++

I will occasionally process spreadsheet with duplicated cells. Let's say when I have multiples like below:

```txt
aaa
bbb
aaa
ccc
aaa
bbb
ccc
ccc
ddd
xxx
aaa
```

I am not a spreadsheet guy by any means, initially I wanted to customize an elisp utility, until I found the killer emacs command: `delete-duplicate-lines`. Just mark all the lines, then hit the command, there is where amazing happens.

```txt
aaa
bbb
ccc
ddd
xxx
```
