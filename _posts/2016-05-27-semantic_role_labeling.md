---
layout: post
title: Semantic Role Labeling
category: Notes
comments: true
---

# Semantic Role Labeling

------

### Semantic Role Labeling

语义角色标注按照现代语法知识将词语序列分组，并按照语义角色对它们进行分类，即对于给定句子中的每个谓词（动词、名词等）分析出其中的相应语义成分，并作相应的语义标记，如施事、受事、工具或附加语等。

Semantic roles are utilized to find concepts automatically and assure their meaningfulness. Semantic role labeling is a research problem which finds in a given sentence the predicates and their arguments (identification), and further labels the semantic relationship between predicates and arguments, that is, their semantic roles (classification).

语义角色标注问题和算法最先由Gildea和Jurafsky于2002年提出。其算法主要基于文法和词汇特征，并转化成分类问题，并通过各种方法解决标注数据的稀疏问题。

In their approach, they emphasize the selection of appropriate lexical and syntactical features for SRL, the use of statistical classifiers and their combinations, and ways to handle data sparseness.

之后的各种尝试也着眼于对特征集进行扩充或改进，以及各种针对稀疏问题进行解决。

Augmenting and/or altering the feature set and attempting different ways to handle data sparseness.

论文"A Dual-Layer Semantic Role Labeling System"提出一种双层语义角色标注算法。

<http://www.aclweb.org/anthology/P15-4009>

系统框架如下图所示。

![此处输入图片的描述][1]

语义角色标注算法等价于分类问题，所基于的特征如下。

Features includes head word related features, target word related features, grammar related features, and semantic type related features.

从句子从抽取的成分可能会有多个，每个成分的角色不同（例如名词分别对应主、宾），角色相互依存，即针对多个成分的角色的分类问题并非相互独立。针对每个成分的角色分类问题从成分对应的序列窗口中抽取特征。

Suppose for any given predicate P in a sentence, the system has identified the three potential arguments A1, A2, and A3 of the predicate, the semantic roles of arguments may depend on each other(argument interdependence).

All the surrounding arguments’ predicted labels are used with window size [-∞,∞]. This also conforms to the rule that when a role is taken by the other argument, it is less likely that the current argument is of the same role.

如下如所示，系统算法分为两份，第一层中首先针对各个成文进行角色分类，此后在第二层中结合相关的其他成分的角色分类结果进行分类。

![此处输入图片的描述][2]

In layer 1 the baseline system is used to predict the labels for identified nodes. Then in layer 2, these predicted labels of all surrounding arguments (in this example, A1 and A3) together with other features of the current node (A2) are used to predict the label of the current node.

之后基于角色标注的结果生成义群（concepts）。义群的生成基于角色的各种组合所形成的模板（templates）。义群的模板如下。

![此处输入图片的描述][3]

The concepts are formulated by concept templates designed according to Propbank SRL labels. These role combinations serve as templates which can capture a complete and important piece of information described in one sentence to form a concept.

系统的抽取的实例如下图所示。

![此处输入图片的描述][4]

***相关连接***

 - http://www.aclweb.org/anthology/P15-4009

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_4.png
