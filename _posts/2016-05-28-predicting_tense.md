---
layout: post
title: Predicting Tense
category: Notes
comments: true
---

# Predicting Tense

------

### Predicting Tense

英文中的时间相对关系能够通过时态确定，而中文中需要通过挖掘语言中显式或隐含表达时间的信息。常用的方法包括基于规则的抽取（关键词表+正则），虽然简单，但由于关键词表的覆盖面比较狭窄，同时很多信息是隐含而非显式的，因此效果欠佳。

Temporal relation resolution involves extraction of temporal information explicitly or implicitly embedded in a language.   
Analyzing temporal structure of a discourse by taking into account the effects of tense, aspect, temporal adverbials and rhetorical relations (e.g. causation and elaboration) on temporal ordering.   
Given a sentence describing two temporally related events, the temporal position words (including before, after and when, which serve as temporal connectives) and the tense/aspect markers of the second event are took into consideration. The proposed rule-based approach was simple; but it suffered from low coverage and was particularly ineffective when the interaction between the linguistic elements was unclear.

如下是归纳的13中时间相对关系（时间点/时间段）。

![此处输入图片的描述][1]

通常为了确定时间相对关系，采用下面的语言学特征。

 - Tense/Aspect in English is manifested by verb inflections.
 - Temporal Connectives in English primarily involve conjunctions, e.g. after, before and when.
 - Event Class is implicit in a sentence. Events can be classified according to their inherent temporal characteristics, such as the degree of telicity and/or atomicity.

论文"Applying Machine Learning to Chinese Temporal Relation Resolution"采用基于特征的机器学习算法对时间关系进行分类。

<http://www.aclweb.org/anthology/P04-1074>

其中所使用的主要特征如下。

![此处输入图片的描述][2]

Linguistic features: eleven temporal indicators and one event class.

时间关系的确定等价为分类问题，该文中采用了两种分类器Probabilistic Decision Tree Classifier和Naïve Bayesian Classifier，然后对结果采用协同提升技术提高准确率。

Model the thirteen relative temporal relations as the classes to be decided by a classifier. Train two classifiers, a Probabilistic Decision Tree Classifier (PDT) and a Naïve Bayesian Classifier (NBC) and combine the results by the Collaborative Bootstrapping (CB) technique.

论文"One Tense per Scene: Predicting Tense in Chinese Conversations"提出了一种将单句（local）的时间信息和整体（global）的时间信息同时考虑提高时态分类准确率的方法。

Propose a set of novel sentence-level (local) features using rich linguistic resources and then propose a new hypothesis of "One tense per scene" to incorporate scene-level (global) evidence to enhance the performance.

算法的核心基于条件随机场（CRF）算法，优化目标如下，其中第一项对应单句的似然（local likelihood），第二项对句间时态的频繁切换进行惩罚（global）。

The goal of this model is to output an optimal sequence t* = {t*1, t*2, ..., t*n} that minimizes the cost function defined as follows:

![此处输入图片的描述][3]

***相关连接***

 - http://www.aclweb.org/anthology/P04-1074
 - http://www.aclweb.org/anthology/P15-2110

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-28-predicting_tense/2016-05-28-predicting_tense_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-28-predicting_tense/2016-05-28-predicting_tense_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-28-predicting_tense/2016-05-28-predicting_tense_3.png
