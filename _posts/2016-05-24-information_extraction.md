---
layout: post
title: Information Extraction
category: Notes
comments: true
---

# Information Extraction

------

### Information Extraction

信息抽取是把文本里包含的信息进行结构化处理。通常信息抽取依赖于文法模式（例如正则），因此通常的处理方法依赖于浅层理解或解析出的依存树。目前主流的Open IE系统包括TEXTRUNNER、WOE、StatSnowBall等。

Open Information Extraction (IE) is the task of extracting assertions from massive corpora without requiring a pre-specified vocabulary.   
Conventionally, open IE systems search a collection of patterns over either the surface form or dependency tree of a sentence.   
Several Open IE systems have been proposed before now, including TEXTRUNNER [Banko et al., 2007], WOE [Wu and Weld, 2010], and StatSnowBall [Zhu et al., 2009]. All these systems use the following three-step method:

主流的Open IE系统的处理过程通常包括以下三个步骤，即打标签、学习和抽取，常用的监督或半监督算法包括：最大熵、条件随机场，以及各种分类和序列标注算法。

 - Label: Sentences are automatically labeled with extractions using heuristics or distant supervision.
 - Learn: A relation phrase extractor is learned using a sequence-labeling graphical model (e.g., CRF).
 - Extract: the system takes a sentence as input, identifies a candidate pair of NP arguments (Arg1, Arg2) from the sentence, and then uses the learned extractor to label each word between the two arguments as part of the relation phrase or not.

主流的信息抽取通过分析句式结构，常根据语法的主、谓、宾或主、系、表将其解析上三元组的形式，为了抽取复杂句中的多个三元组，通常需要首先对复杂句进行拆解成简单句，由于语法成分的关系比较复杂，情况多变，因此成为难点之一。

论文"Leveraging Linguistic Structure For Open Domain Information Extraction"提出了一种基于coherent clauses的句式拆解方法，之后进行角色标注。

<https://web.stanford.edu/~angeli/papers/2015-acl-openie.pdf>

Generating coherent clauses before applying patterns helps reduce false matches such as (Honolulu; be born in; Hawaii).

文中对于coherent clauses的解释如下。

Coherent clauses which are (1) logically entailed by the original sentence, and (2) easy to segment into open IE triples.

该方法首先将sentence分解成shorter utterances，采用natural logic在保持context的条件下最大化的简化句式。之后基于14个人工模式从简单句抽取出三元组。

First learn a classifier for splitting a sentence into shorter utterances, and then appeal to natural logic to maximally shorten these utterances while maintaining necessary context. A small set of 14 hand-crafted patterns can then be used to segment an utterance into an open IE triple.

第一阶段的拆解问题可视为贪婪搜索问题，等价于在依存树结构中搜索哪些子树可以形成简单句。优化的目标是在保证子树句法和语义完整的条件下最大化切分依存树。

Treat the first stage as a greedy search problem: traverse a dependency parse tree recursively, at each step predicting whether an edge should yield an independent clause. The objective is to produce a set of clauses which can stand on their own syntactically and semantically, and are entailed by the original sentence.

整体算法过程如下图所示。

![此处输入图片的描述][1]

From left to right, a sentence yields a number of independent clauses (e.g., she Born in a small town). From top to bottom, each clause produces a set of entailed shorter utterances, and segments the ones which match an atomic pattern into a relation triple

贪婪搜索可以视为对连接结点的边做二元分类：保持/断开。二元分类所采用的特征所示。

Train a multinomial logistic regression classifier on our noisy training data.

The feature class is a high level description of features; the feature templates are the particular templates used. For instance, the POS signature contains the tag of the parent, the tag of the child, and both tags joined in a single feature. Note that all features are joined with the action to be taken on the parent.

![此处输入图片的描述][2]

由此可见，信息抽取中的关键问题是如何避免incoherent extractions and uninformative extractions，解释如下。

Two significant problems in all prior Open IE systems: incoherent extractions and uninformative extractions. Incoherent extractions are cases where the extracted relation phrase has no meaningful interpretation. Uninformative extractions, occurs when extractions omit critical information.

![此处输入图片的描述][3]

针对上述问题，论文"Open Information Extraction The Second Generation"提出了一种基于文法和词汇约束的方法。

<http://turing.cs.washington.edu/papers/etzioni-ijcai2011.pdf>

文法约束是基于词性标注（part-of-speech-based）的正则表达式。之后对抽取出的句式进行统计/合并形成模式，基于模式能够进一步抽取三元组。

文法约束的正则表达式如下。

![此处输入图片的描述][5]

A simple part-of-speech-based regular expression reduces the number of incoherent extractions. If the pattern matches multiple adjacent sequences, they are merged into a single relation phrase.

系统框架如下图所示。

![此处输入图片的描述][4]

针对三元组（文中等价为Relation和Argument）的抽取，系统ARGLEARNER将其拆解为两个子问题，即Arg1和Arg2的定位，定位问题等价为左、后边界的判定/分类问题。

ARGLEARNER divides this task into two subtasks - finding Arg1 and Arg2 - and then subdivides each of these subtasks again into identifying the left bound and the right bound of each argument. ARGLEARNER employs three classifiers to this aim. Two classifiers identify the left and right bounds for Arg1 and the last classifier identifies the right bound of Arg2.

基于模式抽取的类型及实例如下。

![此处输入图片的描述][6]

***相关连接***

 - https://web.stanford.edu/~angeli/papers/2015-acl-openie.pdf
 - https://homes.cs.washington.edu/~soderlan/Fader-emnlp11.pdf
 - http://turing.cs.washington.edu/papers/etzioni-ijcai2011.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-24-information_extraction/2016-05-24-information_extraction_6.png
