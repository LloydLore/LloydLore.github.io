+++
title = "工行融 e 行个人信息保护政策分析"
author = ["丛朝"]
date = 2023-05-29T00:00:00+08:00
lastmod = 2023-05-29T23:56:18+08:00
tags = ["org", "hugo"]
categories = ["terms"]
draft = false
weight = 2001
toc = true
isCJKLanguage = true
+++

> 本文尝试对工行融 e 行的个人信息保护政策做出一些分析，我尝试通过这些分析得出一些关键字。以作为其他分析时的参考。


## 基于功能的数据和基于数据的功能 {#基于功能的数据和基于数据的功能}

从信息安全三位一体的角度上说，我们需要保护的一是服务，二是数据，即 Service &amp; Data。在做个人信息识别，和资产识别时，可以分别从基于功能的数据和基于数据的功能的角度来展开。

打比方，在穷举一个产品具有的所有功能的同时，我们可以将每一个功能用到的数据，特别是个人信息相关的数据整理出来。如此，在产品功能逻辑上，就可以达到全部的覆盖。

从我个人的角度，“基于功能的数据”恐怕是更为适合我自己去识别个人信息以及资产的方式。

那如何理解“基于数据的功能”呢？在一个系统的运行环境中，个人（actor）这是比较容易识别的，而有关个人的数据，在每个评估者那儿都应该有其积累的经验值。根据这个经验值，评估者在运行环境中，可去结合已有的功能分析，更进一步，可以去评估将来可能会有的功能场景。

从实践的角度来看，我认为评估者应当先对系统运行环境进行全方位分析，基于分析结果，评估个人信息及资产。然后再用自己的经验值作为验证。
所以说，基于功能的数据和基于数据的功能，两者在行程安全概念的过程中，相辅相成，缺一不可。


## 工行融 e 行关键字识别 {#工行融-e-行关键字识别}

为了找出这些关键字，我对原文中加粗的字体，做了解析。同时也尝试做了一个词云。执行的代码如下：

```python
import sys
import matplotlib
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from wordcloud import STOPWORDS
from requests_html import HTMLSession

# get links
session = HTMLSession()
url = 'https://m.icbc.com.cn/ICBC/disclaimer/2.htm'
r = session.get(url)

# select the selector
sel = '#infoDetailPage > div.infoText.fs16 > p > strong'
bold_text = r.html.find(sel)

def get_text_from_sel(sel):
    mylist = []
    try:
        results = sel
        for result in results:
            mytext = result.text
            mylist.append(mytext)
        return mylist
    except:
        return None

bold_text_string = ' '.join(get_text_from_sel(bold_text))

# set STOPWORDS
sw = set(STOPWORDS)
sw.add("一")
sw.add("二")
sw.add("三")

wordcloud = WordCloud(scale=4,
                      background_color='white',
                      stopwords=sw,
                      font_path='/usr/share/fonts/OTF/NotoSansCJKsc-Regular.otf').generate(bold_text_string)

# plt, fontset, imshow, etc
from matplotlib import font_manager
font_dirs = '/home/lj/Downloads/transfonter.org-unpack-20230527-152629'
font_files = font_manager.findSystemFonts(fontpaths=font_dirs)

for font_file in font_files:
    font_manager.fontManager.addfont(font_file)

# %matplotlib inline
plt.figure(dpi=300, figsize=(24,8))
matplotlib.rcParams['font.family'] = 'WenQuanYi Zen Hei'
plt.title(label='常见水果热力图')
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
# plt.show()
fname = '/home/lj/Pictures/icbc_pii_bold_text.png'
plt.savefig(fname)
fname
```
