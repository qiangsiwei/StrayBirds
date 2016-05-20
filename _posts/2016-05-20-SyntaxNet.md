---
layout: post
title: SyntaxNet原理分析(2)
category: Notes
comments: true
---

# SyntaxNet原理分析(2)

------

### SyntaxNet原理分析(2)

SyntaxNet作为一个parser（NLP系统中的关键组件之一），能够针对系统中输入一个句子，自动给句子中的每一个单词打上POS（part-of-Speech）标签，用来描述这些词的句法功能，之后在依存句法树中呈现。这些句法关系直接涉及句子的潜在含义。

SyntaxNet基于arc-standard system（one of the most popular transition systems）实现。arc-standard system的介绍可参考：SyntaxNet原理分析(1)。

![此处输入图片的描述][1]

![此处输入图片的描述][2]

![此处输入图片的描述][3]

![此处输入图片的描述][4]

![此处输入图片的描述][5]

![此处输入图片的描述][6]

![此处输入图片的描述][7]

![此处输入图片的描述][8]

![此处输入图片的描述][9]

![此处输入图片的描述][10]

***相关连接***

 - https://github.com/qiangsiwei/models/tree/master/syntaxnet


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_3.gif
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-20-SyntaxNet/2016-05-20-SyntaxNet_11.png

