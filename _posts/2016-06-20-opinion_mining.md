---
layout: post
title: Opinion Mining
category: Notes
comments: true
---

# Opinion Mining

------

### Opinion Mining

互联网包含着大量的非结构化文本信息, 分析这些文本信息是非常重要的。观点挖掘非常具有挑战性, 然而它有广阔的应用前景。

比如各公司总是希望能够及时获取公众或者消费者对于它们产品和服务的评价, 以便进一步改进 这些产品和服务。

## Different Levels of Analysis

观点挖掘具有不同的级别，包括文档、句子、观点等。

 - Document level: The task at this level is to classify whether a whole opinion document expresses a positive or negative sentiment.
 - Sentence level: The task at this level goes to the sentences and determines whether each sentence expressed a positive, negative, or neutral opinion. Neutral usually means no opinion.
 - Entity and Aspect level: Both the document level and the sentence level analyses do not discover what exactly people liked and did not like. Aspect level performs finer-grained analysis.

## Sentiment Lexicon and Its Issues

观点挖掘中情感极性的判别通常会依靠情感极性词典，然后单纯依靠词典将产生以下几方面问题。

 - A positive or negative sentiment word may have opposite orientations in different application domains.
 - A sentence containing sentiment words may not express any sentiment. This phenomenon happens frequently in several types of sentences. Question (interrogative) sentences and conditional sentences are two important types.
 - Sarcastic sentences with or without sentiment words are hard to deal
with, e.g., "What a great car! It stopped working in two days."
 - Many sentences without sentiment words can also imply opinions. Many of these sentences are actually objective sentences that are used to express some factual information.

## The Problem of Sentiment Analysis

情感分析问题的描述如下。

Definition (Opinion): An opinion is a quadruple,   
(g, s, h, t),   
where g is the opinion (or sentiment) target, s is the sentiment about the target, h is the opinion holder and t is the time when the opinion was expressed.

情感分析问题中关键的两个要素是对象（entity）和观点（opinion），其定义如下。

Definition (entity): An entity e is a product, service, topic, issue, person, organization, or event. It is described with a pair, e: (T, W), where T is a hierarchy of parts, sub-parts, and so on, and W is a set of attributes of e.

Definition (opinion): An opinion is a quintuple,   
(ei, aij, sijkl, hk, tl),   
where ei is the name of an entity, aij is an aspect of ei, sijkl is the sentiment on aspect aij of entity ei, hk is the opinion holder, and tl is the time when the opinion is expressed by hk. The sentiment sijkl is positive, negative, or neutral, or expressed with different strength/intensity levels, e.g., 1 to 5 stars as used by most review sits on the Web. When an opinion is on the entity itself as a whole, the special aspect GENERAL is used to denote it. Here, ei and aij together represent the opinion target.

## Sentiment Analysis Tasks

情感分析的主要任务是从文档中挖掘出上述的5元组。

Objective of sentiment analysis: Given an opinion document d, discover all opinion quintuples (ei, aij, sijkl, hk, tl) in d.

相关的定义如下。

 - Definition (entity category and entity expression): An entity category represents a unique entity, while an entity expression is an actual word or phrase that appears in the text indicating an entity category.
 - Definition (aspect category and aspect expression): An aspect category of an entity represents a unique aspect of the entity, while an aspect expression is an actual word or phrase that appears in the text indicating an aspect category.
 - Definition (explicit aspect expression): Aspect expressions that are nouns and noun phrases are called explicit aspect expressions.
 - Definition (implicit aspect expression): Aspect expressions that are not nouns or noun phrases are called implicit aspect expressions.

情感分析的子任务主要包括以下6个。

 - Task 1 (entity extraction and categorization): Extract all entity expressions in D, and categorize or group synonymous entity expressions into entity clusters (or categories). Each entity expression cluster indicates a unique entity ei.
 - Task 2 (aspect extraction and categorization): Extract all aspect expressions of the entities, and categorize these aspect expressions into clusters. Each aspect expression cluster of entity ei represents a unique aspect aij.
 - Task 3 (opinion holder extraction and categorization): Extract opinion holders for opinions from text or structured data and categorize them. The task is analogous to the above two tasks.
 - Task 4 (time extraction and standardization): Extract the times when opinions are given and standardize different time formats. The task is also analogous to the above tasks.
 - Task 5 (aspect sentiment classification): Determine whether an opinion on an aspect aij is positive, negative or neutral, or assign a numeric sentiment rating to the aspect.
 - Task 6 (opinion quintuple generation): Produce all opinion quintuples (ei, aij, sijkl, hk, tl) expressed in document d based on the results of the above tasks. This task is seemingly very simple but it is in fact very difficult in many cases as Example 4 below shows.

## Different Types of Opinions

观点的分类如下，包括直接观点、隐含观点、对比观点等。

Regular opinion: A regular opinion is often referred to simply as an opinion in the literature and it has two main sub-types (Liu, 2006 and 2011):

  - Direct opinion: A direct opinion refers to an opinion expressed directly on an entity or an entity aspect, e.g., "The picture quality is great." 
  - Indirect opinion: An indirect opinion is an opinion that is expressed indirectly on an entity or aspect of an entity based on its effects on some other entities. This sub-type often occurs in the medical domain. For example, the sentence "After injection of the drug, my joints felt worse" describes an undesirable effect of the drug on "my joints", which indirectly gives a negative opinion or sentiment to the drug. In the case, the entity is the drug and the aspect is the effect on joints.

Comparative opinion: A comparative opinion expresses a relation of similarities or differences between two or more entities and/or a preference of the opinion holder based on some shared aspects of the entities (Jindal and Liu, 2006a; Jindal and Liu, 2006b).

Explicit opinion: An explicit opinion is a subjective statement that gives a regular or comparative opinion.

Implicit (or implied) opinion: An implicit opinion is an objective statement
that implies a regular or comparative opinion.

## Aspect-based Sentiment Analysis

做情感分析前首先要找出某观点针对什么目标所发，例如，目标可以是某产品、产品的部件、产品的特征、产品的属性等等，上述目标也可以推及商品、服务、个人、组织、事件、话题等等。

因此，Aspect-based Sentiment Analysis问题，除了需要确定情感极性，还需要确定情感所对应的目标（Aspect）。

Classifying opinion texts at the document level or the sentence level is often insufficient for applications because they do not identify opinion targets or assign sentiments to such targets. Even if we assume that each document evaluates a single entity, a positive opinion document about the entity does not mean that the author has positive opinions about all aspects of the entity. Likewise, a negative opinion document does not mean that the author is negative about everything. For more complete analysis, we need to discover the aspects and determine whether the sentiment is positive or negative on each aspect.

At the aspect level, the objective is to discover every quintuple (ei, aij, sijkl, hk, tl) in a given document d. To achieve this goal, six tasks have to be performed.

Aspect-based Sentiment Analysis问题主要包括两个子问题，即Aspect extraction和Aspect sentiment classification。

###  Aspect sentiment classification

This task determines whether the opinions on different aspects are positive, negative, or neutral. In the first example above, the opinion on the "voice quality" aspect is positive. In the second, the opinion on the aspect GENERAL is also positive.

There are two main approaches for determining the orientation of sentiment expressed on each aspect in a sentence: the supervised learning approach and the lexicon-based approach.

情感极性的判别方法主要包括基于词典的方法（lexicon-based method）和基于语义规则的方法（Basic Rules of Opinions and Compositional Semantics）。

#### lexicon-based method

基于词典的方法主要包含以下几个步骤。

 - Mark sentiment words and phrases: For each sentence that contains one or more aspects, this step marks all sentiment words and phrases in the sentence. Each positive word is assigned the sentiment score of +1 and each negative word is assigned the sentiment score of 􏰉1. 
 - Apply sentiment shifters: Sentiment shifters (also called valence shifters in (Polanyi and Zaenen, 2004)) are words and phrases that can change sentiment orientations.
 - Handle but-clauses: Words or phrases that indicate contrary need special handling because they often change sentiment orientations too.
 - Aggregate opinions: This step applies an opinion aggregation function to the resulting sentiment scores to determine the final orientation of the sentiment on each aspect in the sentence.

#### Basic Rules of Opinions and Compositional Semantics

An opinion rule expresses a concept that implies a positive or negative sentiment. It can be as simple as individual sentiment words with their implied sentiments or compound expressions that may need commonsense or domain knowledge to determine their orientations.

基于语义规则的方法所采用的规则类似于BNF，其表述如下。

The rules are presented using a formalism similar to the BNF form. The rules are from (Liu, 2010).

![此处输入图片的描述][1]

 - Sentiment word or phrase

![此处输入图片的描述][2]

 - Decreased and increased quantity of an opinionated item (N and P)

![此处输入图片的描述][3]

 - High, low, increased and decreased quantity of a positive or negative potential item

![此处输入图片的描述][4]

- Desirable or undesirable fact

![此处输入图片的描述][5]

- Deviation from the norm or a desired value range

![此处输入图片的描述][6]

- Produce and consume resource and waste

![此处输入图片的描述][7]

### Aspect Extraction

Aspect-based Sentiment Analysis问题的另一子问题是Aspect Extraction，然而并不是所有的情感都有明确指向，一方面情感的指向可能是隐含的（谓词隐含了主或宾），另一方面可能是对事物的笼统层面（the GENERAL aspect）。

This task extracts aspects that have been evaluated. For example, in the sentence, "The voice quality of this phone is amazing," the aspect is "voice quality" of the entity represented by "this phone." Note that "this phone" does not indicate the aspect GENERAL here because the evaluation is not about the phone as a whole, but only about its voice quality. However, the sentence "I love this phone" evaluates the phone as a whole, i.e., the GENERAL aspect of the entity represented by "this phone." Bear in mind whenever we talk about an aspect, we must know which entity it belongs to. In our discussion below, we often omit the entity just for simplicity of presentation.

The key characteristic is that an opinion always has a target. The target is often the aspect or topic to be extracted from a sentence. Thus, it is important to recognize each opinion expression and its target from a sentence. However, we should also note that some opinion expressions can play two roles, i.e., indicating a positive or negative sentiment and implying an (implicit) aspect (target).

Aspect Extraction主要包含下面几种方法，具体描述可参考后续文章。

There are four main approaches:

 - Extraction based on frequent nouns and noun phrases
 - Extraction by exploiting opinion and target relations
 - Extraction using supervised learning
 - Extraction using topic modeling

***相关连接***

 - https://www.cs.uic.edu/~liub/FBS/SentimentAnalysis-and-OpinionMining.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-20-opinion_mining/2016-06-20-opinion_mining_7.png
