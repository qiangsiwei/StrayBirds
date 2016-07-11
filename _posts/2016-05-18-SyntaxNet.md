---
layout: post
title: SyntaxNet原理分析(1)
category: Systems
comments: true
---

# SyntaxNet原理分析(1)

------

### SyntaxNet原理分析(1)

SyntaxNet作为一个parser（NLP系统中的关键组件之一），能够针对系统中输入一个句子，自动给句子中的每一个单词打上POS（part-of-Speech）标签，用来描述这些词的句法功能，之后在依存句法树中呈现。这些句法关系直接涉及句子的潜在含义。

SyntaxNet基于arc-standard system（one of the most popular transition systems）实现。为了帮助理解，首先介绍"A Fast and Accurate Dependency Parser using Neural Networks"中基于arc-standard system的基本系统结构。

论文的发表在EMNLP 2014。<http://cs.stanford.edu/people/danqi/papers/emnlp2014.pdf>

在arc-standard system中，一个configuration c = (s,b,A)由一个栈区（stack s），一个缓冲区（buffer b），和一组依存弧（dependency arcs A）组成。初始状态下，栈区仅包含根结点ROOT，依存弧为空，文本全部处于缓冲区。随着文本不断从左至右读入，arc-standard system可以进行以下三种操作，并最终清空缓冲区，并合并栈区中的结点（仅存ROOT结点），由依存弧组成树形结构，构成依存树。

![此处输入图片的描述][1]

通常为了为句子中的每个单词打上POS标签，会考虑以下特征，即当前词的左右邻单词、单词的标签、单词的组合等，对应的模型可能为条件随机场（CRF）等。但基于此种人工选取的特征将存在如下三个问题。

 - 数据稀疏性（Sparsity）。是指文本特征所固有的稀疏性。高阶（组合）特征受限于训练语料库更加稀疏。

 - 特征不完整性（Incompleteness）。特征不完整性是所有基于特征模板（feature templates）的共性问题，特征模板的人工构建将造成很多有益特征的缺失。

 - 特征计算高成本（Expensive feature computation）。基于特征模板构建特征集通常需要从特征表中进行匹配，具有很高的计算高成本，可能占用了解析（parsing）总时间的95%以上。

![此处输入图片的描述][2]

基于transition的system能够有效避免这些问题。为便于理解，下图说明了arc-standard system对一个句子的解析（状态转移）全过程。

![此处输入图片的描述][3]

下图是神经元网络的架构。与单纯的word embedding不同，在该方法中除了对单词做低维嵌入，还同时对POS tags和arc labels也做嵌入，好处在与能够更好的对POS tags和arc labels的相似性进行建模。

通过实验能够对嵌入后的相似性进行解释说明（It clearly shows that these embeddings effectively exhibit the similarities between POS tags or arc labels. For instance, the three adjective POS tags JJ, JJR, JJS have very close embeddings, and also the three labels representing clausal comple- ments acomp, ccomp, xcomp are grouped together）。

而激发函数选择了立方激发函数（cube activation function）。

基于此，原问题可以转化成在当前configuration下，三类transition的预测问题。神经元网络的输入特征是栈区和缓冲区内word、POS、arc label三类特征向量（We choose a set of elements based on the stack / buffer positions for each type of information (word, POS or label), which might be useful for our predictions）。

![此处输入图片的描述][4]

立方激发函数与其他几种激发函数的比较如下图所示。

![此处输入图片的描述][5]

如下式说明了立方激发函数的优势，能够更完整的捕捉三类特征之间的组合关系。

![此处输入图片的描述][6]

如下图说明了对POS tags和arc labels低维嵌入的结果，实际结果基于t-SNE（van der Maaten and Hinton, 2008）进行可视化。Since these embeddings can effectively encode the semantic regularities, we believe that they can be also used as alternative features of POS tags (or arc labels) in other NLP tasks, and help boost the performance.

![此处输入图片的描述][7]


***相关连接***

 - https://github.com/qiangsiwei/models/tree/master/syntaxnet


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-18-SyntaxNet/2016-05-18-SyntaxNet_7.png

