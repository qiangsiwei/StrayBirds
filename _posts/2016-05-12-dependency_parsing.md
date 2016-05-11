---
layout: post
title: 依存语法ACL论文笔记(1)
category: Notes
comments: true
---

# 依存语法ACL论文笔记(1)

------

### 依存语法ACL论文笔记(1)

## **A Re-ranking Model for Dependency Parser with Recursive Convolutional Neural Network**

本文解决基于词和短语的依存语法树计算问题。Address the problem to model all the nodes (words or phrases) in a dependency tree with the dense representations.

提出了一种递归卷积神经网络结构获取依存语法树中词和短语的表征计算。Propose a recursive convolutional neural network (RCNN) architecture to capture syntac- tic and compositional-semantic representations of phrases and words in a dependency tree.

类似CNN主要包含卷积层和池化层。Introduce the convolution and pooling layers, which can model a variety of compositions by the feature maps and choose the most informative compositions by the pooling layers.

为了避免维度灾难，使用嵌入进行降低维。Distributed representations are to represent words (or phrase) by the dense, low-dimensional and real-valued vectors, which help address the curse of dimensionality and have better general- ization than discrete representations.

![此处输入图片的描述][1]

由于依存语法也是递归结构，因此适合采用递归神经元网络。Since the dependency tree is also in recursive structure, it is intuitive to use the recursive neural network (RNN), which is used for constituent parsing (Socher et al., 2013a).

不同的是依存语法每个结点可以有多个子结点。However, recursive neural network can only process the binary combination and is not suitable for dependency parsing, since a parent node may have two or more child nodes in dependency tree.

对于每个结点，首先采用RCNN计算其与每个子结点的值，取信息量最大的进行池化。 For each node in a given dependency tree, we first use a RCNN unit to model the interactions between it and each of its children and choose the most informative features by a pooling layer.

RCNN结构包括以下几个部分：Word Embeddings，Distance Embeddings，Convolution。其中，Distance Embeddings用于计算父节点和每个子结点相对距离的嵌入。

![此处输入图片的描述][2]

卷积的计算过称为：   
![此处输入图片的描述][3]   
![此处输入图片的描述][4]   

池化的计算过称为：   
![此处输入图片的描述][5]   

解析过程（Parsering）最大化所有隐结点的score function。   
![此处输入图片的描述][6]   
![此处输入图片的描述][7]   

为了提高准确度还能与其他parser结合使用，获取得分最高的k个依存树。Use the popular mixture re-ranking strategy (Hayashi et al., 2013; Le and Mikolov, 2014), which is a combination of the our model and the base parser.

## **Dependency-based Convolutional Neural Networks for Sentence Embedding**

该文提出了一种基于依存语法的卷积神经网络以句子为粒度做嵌入，提出的神经元网络为DCNNs(simple dependency-based convolutional neural networks。

卷积神经网络CNN通常用于图像处理，由于仅对局部做卷积，忽略了长期依赖（例如英文中的从句），因此用于自然语言处理有一定局限性。CNNs, being invented on pixel matrices in image processing, only consider sequential n-grams that are consecutive on the surface string and neglect long-distance dependencies, while the latter play an important role in many linguistic phenomena such as negation, subordination, and wh-extraction, all of which might dully affect the sentiment, subjectivity, or other categorization of the sentence.

直接对N元依存树做计算，数据非常稀疏，所以本文首先对数据做嵌入，达到降维的目的。data sparsity (according to our experiments, tree n-grams are significantly sparser than surface n-grams), but this problem has largely been alleviated by the recent advances in word embedding.

该文模型和前人工作的最大区别在于基于依存语法，而非字符序列，因此能够实现长期依赖。Our model is similar to Kim (2014), but while his sequential CNNs put a word in its sequential context, ours considers a word and its parent, grand-parent, great-grand-parent, and siblings on the dependency tree.

依存树结构的简单例子如下。   
![此处输入图片的描述][8]   

字符序列直接做连接如下。   
![此处输入图片的描述][9]   

基于依存树父子递归关系做连接如下。   
![此处输入图片的描述][10]   
![此处输入图片的描述][11]   
![此处输入图片的描述][12]   

神经元网络结点计算如下。   
![此处输入图片的描述][13]   

最终按照字符串序列生成一系列特征。   
![此处输入图片的描述][14]   

***相关连接***

 - <http://www.aclweb.org/anthology/P15-1112>
 - <http://www.aclweb.org/anthology/P15-2029>


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_11.png
  [12]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_12.png
  [13]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_13.png
  [14]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-12-dependency_parsing/2016-05-12-dependency_parsing_14.png

  