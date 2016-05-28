---
layout: post
title: Predicting Tense
category: Notes
comments: true
---

# Predicting Tense

------

### Predicting Tense

Temporal relation resolution involves extraction of temporal information explicitly or implicitly embedded in a language.

Analyzing temporal structure of a discourse by taking into account the effects of tense, aspect, temporal adverbials and rhetorical relations (e.g. causation and elaboration) on temporal ordering.

Given a sentence describing two temporally related events, the temporal position words (including before, after and when, which serve as temporal connectives) and the tense/aspect markers of the second event are took into consideration. The proposed rule-based approach was simple; but it suffered from low coverage and was particularly ineffective when the interaction between the linguistic elements was unclear.

Thirteen temporal relations between points and intervals.

![此处输入图片的描述][1]

Linguistic Features for Determining Relative Relations:

 - Tense/Aspect in English is manifested by verb inflections.
 - Temporal Connectives in English primarily involve conjunctions, e.g. after, before and when.
 - Event Class is implicit in a sentence. Events can be classified according to their inherent temporal characteristics, such as the degree of telicity and/or atomicity.

Linguistic features: eleven temporal indicators and one event class.

![此处输入图片的描述][2]

Model the thirteen relative temporal relations as the classes to be decided by a classifier. Train two classifiers, a Probabilistic Decision Tree Classifier (PDT) and a Naïve Bayesian Classifier (NBC) and combine the results by the Collaborative Bootstrapping (CB) technique.


Propose a set of novel sentence-level (local) features using rich linguistic resources and then propose a new hypothesis of "One tense per scene" to incorporate scene-level (global) evidence to enhance the performance.

The goal of this model is to output an optimal sequence t* = {t*1, t*2, ..., t*n} that minimizes the cost function defined as follows:

![此处输入图片的描述][3]

Global tense prediction.

![此处输入图片的描述][4]


***相关连接***

 - http://www.aclweb.org/anthology/P04-1074
 - http://www.aclweb.org/anthology/P15-2110

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-29-predicting_tense/2016-05-29-predicting_tense_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-29-predicting_tense/2016-05-29-predicting_tense_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-29-predicting_tense/2016-05-29-predicting_tense_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-29-predicting_tense/2016-05-29-predicting_tense_4.png
  