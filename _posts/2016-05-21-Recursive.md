---
layout: post
title: Recursive Neutral Network
category: Notes
comments: true
---

# Recursive Neutral Network

------

### Recursive Neutral Network

结构递归神经网络（Recursive Neutral Network）是一类用结构递归的方式构建的网络，例如递归自编码机（Recursive Autoencoder or RAE），常用于在自然语言处理的神经网络分析方法中用于解析语句。

递归神经元网络可以用于多种场合，例如NLP、computer vision等。自然语言通常可以解析成为递归结构（例如英文中的从句和引导词修饰词之间的关系）。The syntactic rules of natural language are known to be recursive, with noun phrases containing relative clauses that themselves contain noun phrases. 递归神经元网络具有表达组合和相似的内在联系（part-of and proximity relationships）。


![此处输入图片的描述][1]

针对递归神经网络可以采用Max-Margin Estimation以优化目标进行参数估计。针对递归结构的判断的正确性，以一下函数进行误差估计（即计算出的子树是否包含于标注的子树）。

![此处输入图片的描述][2]

针对训练集以以下函数作为目标函数优化模型参数。其中s相当于模型的loss得分（越高越好）。

![此处输入图片的描述][3]

如下图所示，针对图像中的像素点或字符串中的字符序列，根据相邻关系定义出临接矩阵（adjacency matrix）。只有临近的结点能够进行递归。A training image (red and blue are differently labeled regions) defines a set of correct trees which is oblivious to the order in which segments with the same label are merged.

![此处输入图片的描述][4]

加入正则项（避免过拟合）后的整体优化目标如下（regularized risk function）。

![此处输入图片的描述][5]

下图所示是一个典型的递归神经元微观结构。针对N个结点，需要经过N-1次递归结合。不同于一般的递归神经网络，该模型同时输出输入的两个结点是否能够结合。One recursive neural network which is replicated for each pair of possible input vectors. This network is di↵erent to the original RNN formulation in that it predicts a score for being a correct merging decision.

![此处输入图片的描述][6]

下面是递归的计算过程，每次结合两个相邻结点，并重新加入到待选集合。Let the score sij be the highest score.

 - Remove [ai,aj] from C, as well as all other pairs with either ai or aj in them. 
 - Update the adjacency matrix with a new row and column that reflects that the new segment has the neighbors of both child segments. 
 - Add potential child pairs to C.

![此处输入图片的描述][7]

由于hinge loss，regularized risk function不可微，提出了一种subgradient的方法（computes a gradient-like direction），则梯度计算如下。

![此处输入图片的描述][8]

由此，基于递归神经网络所有树结点反传误差对参数的梯度计算如下。

![此处输入图片的描述][9]

两种基于自动编码器的典型递归神经网络（微观结构）如下。不同的是误差反传的层级不同。更详细的分析和实现请参考后续更新。

![此处输入图片的描述][10]

------

RAE的实现代码搜集如下。

 - Numpy   
<https://gist.github.com/ktnyt/c426a0a0da1180fd8e48>

 - Theano   
<https://github.com/shawntan/rnn-experiment/blob/master/rae.py>   
<https://blog.wtf.sg/2014/05/12/recursive-auto-encoders-example-in-theano/>

 - str2vec   
<https://github.com/pengli09/str2vec>   
str2vec is a toolkit for computing vector-space representations for variable-length phrases using recursive autoencoders (RAE)   
Reference: Peng Li, Yang Liu, Maosong Sun. Recursive Autoencoders for ITG-based Translation. Proc. of EMNLP 2013: Conference on Empirical Methods in Natural Language Processing, Seattle, Washington, USA, 2013, pp. 567-577.

 - Implementation by Socher (Matlab)   
<http://www.socher.org/index.php/Main/Semi-SupervisedRecursiveAutoencodersForPredictingSentimentDistributions>   
<http://www.socher.org/index.php/Main/DynamicPoolingAndUnfoldingRecursiveAutoencodersForParaphraseDetection>

------

顺便搜集了一下Deep Learning大牛们的主页或blog。

 - Geoffrey Hinton
<http://www.cs.toronto.edu/~hinton/>

 - Yann LeCun
<http://yann.lecun.com/>

 - Yoshua Bengio
<http://www.iro.umontreal.ca/~bengioy/yoshua_en/index.html>

 - Andrew Ng
<http://www.andrewng.org/>

 - Chris Manning
<http://nlp.stanford.edu/~manning/>

 - Richard Socher
<http://www.socher.org/index.php/Main/HomePage>

 - Honglak Lee
<http://web.eecs.umich.edu/~honglak/>

 - Rajat Raina
<http://ai.stanford.edu/~rajatr/research.html>


***相关连接***

 - http://www.socher.org/
 - http://nlp.stanford.edu/~socherr/thesis.pdf


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_10.png
