---
layout: post
title: Aspect-based Sentiment Analysis
category: Notes
comments: true
---

# Aspect-based Sentiment Analysis

------

### Aspect-based Sentiment Analysis

With the proliferation of user-generated content, interest in mining sentiment and opinions in text has grown rapidly, both in academia and business. The majority of current approaches, however, attempt to detect the overall polarity of a sentence, paragraph, or text span, irrespective of the entities mentioned (e.g., laptops, battery, screen) and their attributes (e.g. price, design, quality).

## Aspect-based Sentiment Analysis System

<http://www.aclweb.org/anthology/P15-4010>

The input to the pipeline is a string of Chinese characters; the output is a set of relationships between evaluations and their targets. The main goal is to demonstrate how knowledge about sen- tence structure can increase the precision, insight value and granularity of the output.

Formulate the task of sentiment analysis in two steps, namely unit identification and relation extraction.

The presented model is complemented by a probabilistic model which performs topic and polarity classification on the sentence and the document levels.

The basic assumption on which the model builds is that language follows rules. Chinese phrase struc- ture is largely head-final (Huang 1982, Li 1990, i. a.): nominal modifiers precede their head nouns, whereas degree and negation adverbs normally precede the adjectives or verbs they modify. A small set of correspond- ing phrase-level rules achieves a high coverage. 

The goal of aspect-based sentiment analysis is to derive the opinions of a speaker about an entity and its features.

Given an opinion statement on a specific product, translate the statement into a set of (f eature, < evaluation|emotion > ) pairs in two processing steps:

![此处输入图片的描述][1]

It has long been observed that many evaluation words come with implicit features; for example, the evaluation beautiful implicitly contains the feature VisualAppearance. In order to preserve this meaning, we adopt a scalar representation of evaluations (cf. Kennedy and McNally (2005) for a linguistic analysis of scalar expressions): evaluations are represented as pairs of a feature and a numerical value which "maps" the evaluation to some point on the feature scale [-3, 3].

The final mapping goes from sequences of features to numerical evaluations.

![此处输入图片的描述][2]

![此处输入图片的描述][3]

### Lexical basis

Semantic categories are relevant for the interpretation of opinions. The top-level semantic categories are:

![此处输入图片的描述][4]

### Processing steps

![此处输入图片的描述][5]

Use a phrase rule grammar which is based on regular expressions involving the POS tags of the words

![此处输入图片的描述][6]

In the following, present some of the most common phrase structures for features and evalu- ations/emotions.

#### Feature phrases

![此处输入图片的描述][7]

#### Evaluation and emotion chunks

![此处输入图片的描述][8]

#### Relation extraction

The causer relation. The causer relation is a fairly well-delimited relation which describes the causer of some state of event.

![此处输入图片的描述][9]

The theme relation. The theme relation is ex- pressed differently for evaluations and emotions.

![此处输入图片的描述][10]

![此处输入图片的描述][11]

## SemEval-2015 Task 12

SemEval-2015 Task 12: Aspect Based Sentiment Analysis

In aspect-based sentiment analysis (ABSA) the aim is to identify the aspects of entities and the sentiment expressed for each aspect. The ultimate goal is to be able to generate summaries listing all the aspects and their overall polarity such as the example shown in Fig.

![此处输入图片的描述][12]

In addition, SE-ABSA15 will include an out-of-domain ABSA subtask, involving test data from a domain unknown to the participants, other than the domains that will be considered during training. In particular, SE-ABSA15 consists of the following two subtasks.

### Subtask 1: In-domain ABSA
 
Given a review text about a laptop or a restaurant, identify the following types of information:
 
 - Slot 1: Aspect Category (Entity and Attribute). Identify every entity E and attribute A pair E#A towards which an opinion is expressed in the given text.
 - Slot 2 (Only for the restaurants domain): Opinion Target Expression (OTE). An opinion target expression (OTE) is an expression used in the given text to refer to the reviewed entity E of a pair E#A. 
 - Slot 3: Sentiment Polarity. Each identified E#A pair of the given text has to be assigned a polarity, from a set P = {positive, negative, neutral}.

### Subtask 2: Out-of-domain ABSA

The input for the participating systems will be reviews from a particular domain (e.g., laptop or restaurant reviews). Each system will be required to construct all the {E#A, P} tuples for the laptops domain and the {E#A, OTE, P} tuples for the restaurants domain.

### Datasets

Two datasets of ~550 reviews of laptops and restaurants annotated with opinion tuples (as shown in Fig. 2) will be provided for training. Additional datasets will be provided to evaluate the participating systems in Subtask 1 (in-domain ABSA). Information about the domain adaptation dataset of Subtask 2 (out-of-domain ABSA) will be provided in advance.

***相关连接***

 - http://www.aclweb.org/anthology/P15-4010
 - http://alt.qcri.org/semeval2015/task12/
 - http://alt.qcri.org/semeval2016/task5/
 - https://www.cs.uic.edu/~liub/FBS/SentimentAnalysis-and-OpinionMining.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_11.png
  [12]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-14-aspect_setiment/2016-06-14-aspect_setiment_12.png
