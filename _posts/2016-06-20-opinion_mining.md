---
layout: post
title: 
category: Notes
comments: true
---

# Opinion Mining

------

### Opinion Mining

## Different Levels of Analysis

 - Document level: The task at this level is to classify whether a whole opinion document expresses a positive or negative sentiment.
 - Sentence level: The task at this level goes to the sentences and determines whether each sentence expressed a positive, negative, or neutral opinion. Neutral usually means no opinion.
 - Entity and Aspect level: Both the document level and the sentence level analyses do not discover what exactly people liked and did not like. Aspect level performs finer-grained analysis.

## Sentiment Lexicon and Its Issues

 - A positive or negative sentiment word may have opposite orientations in different application domains.
 - A sentence containing sentiment words may not express any sentiment. This phenomenon happens frequently in several types of sentences. Question (interrogative) sentences and conditional sentences are two important types.
 - Sarcastic sentences with or without sentiment words are hard to deal
with, e.g., "What a great car! It stopped working in two days."
 - Many sentences without sentiment words can also imply opinions. Many of these sentences are actually objective sentences that are used to express some factual information.

## The Problem of Sentiment Analysis

Definition (Opinion): An opinion is a quadruple,   
(g, s, h, t),   
where g is the opinion (or sentiment) target, s is the sentiment about the target, h is the opinion holder and t is the time when the opinion was expressed.

Definition (entity): An entity e is a product, service, topic, issue, person, organization, or event. It is described with a pair, e: (T, W), where T is a hierarchy of parts, sub-parts, and so on, and W is a set of attributes of e.

Definition (opinion): An opinion is a quintuple,   
(ei, aij, sijkl, hk, tl),   
where ei is the name of an entity, aij is an aspect of ei, sijkl is the sentiment on aspect aij of entity ei, hk is the opinion holder, and tl is the time when the opinion is expressed by hk. The sentiment sijkl is positive, negative, or neutral, or expressed with different strength/intensity levels, e.g., 1 to 5 stars as used by most review sits on the Web. When an opinion is on the entity itself as a whole, the special aspect GENERAL is used to denote it. Here, ei and aij together represent the opinion target.

## Sentiment Analysis Tasks

Objective of sentiment analysis: Given an opinion document d, discover all opinion quintuples (ei, aij, sijkl, hk, tl) in d.

 - Definition (entity category and entity expression): An entity category represents a unique entity, while an entity expression is an actual word or phrase that appears in the text indicating an entity category.
 - Definition (aspect category and aspect expression): An aspect category of an entity represents a unique aspect of the entity, while an aspect expression is an actual word or phrase that appears in the text indicating an aspect category.
 - Definition (explicit aspect expression): Aspect expressions that are nouns and noun phrases are called explicit aspect expressions.
 - Definition (implicit aspect expression): Aspect expressions that are not nouns or noun phrases are called implicit aspect expressions.

 - Task 1 (entity extraction and categorization): Extract all entity expressions in D, and categorize or group synonymous entity expressions into entity clusters (or categories). Each entity expression cluster indicates a unique entity ei.
 - Task 2 (aspect extraction and categorization): Extract all aspect expressions of the entities, and categorize these aspect expressions into clusters. Each aspect expression cluster of entity ei represents a unique aspect aij.
 - Task 3 (opinion holder extraction and categorization): Extract opinion holders for opinions from text or structured data and categorize them. The task is analogous to the above two tasks.
 - Task 4 (time extraction and standardization): Extract the times when opinions are given and standardize different time formats. The task is also analogous to the above tasks.
 - Task 5 (aspect sentiment classification): Determine whether an opinion on an aspect aij is positive, negative or neutral, or assign a numeric sentiment rating to the aspect.
 - Task 6 (opinion quintuple generation): Produce all opinion quintuples (ei, aij, sijkl, hk, tl) expressed in document d based on the results of the above tasks. This task is seemingly very simple but it is in fact very difficult in many cases as Example 4 below shows.

## Different Types of Opinions

Regular opinion: A regular opinion is often referred to simply as an opinion in the literature and it has two main sub-types (Liu, 2006 and 2011):

  - Direct opinion: A direct opinion refers to an opinion expressed directly on an entity or an entity aspect, e.g., "The picture quality is great." 
  - Indirect opinion: An indirect opinion is an opinion that is expressed indirectly on an entity or aspect of an entity based on its effects on some other entities. This sub-type often occurs in the medical domain. For example, the sentence "After injection of the drug, my joints felt worse" describes an undesirable effect of the drug on "my joints", which indirectly gives a negative opinion or sentiment to the drug. In the case, the entity is the drug and the aspect is the effect on joints.

Comparative opinion: A comparative opinion expresses a relation of similarities or differences between two or more entities and/or a preference of the opinion holder based on some shared aspects of the entities (Jindal and Liu, 2006a; Jindal and Liu, 2006b).

Explicit opinion: An explicit opinion is a subjective statement that gives a regular or comparative opinion.

Implicit (or implied) opinion: An implicit opinion is an objective statement
that implies a regular or comparative opinion.

## Aspect-based Sentiment Analysis

Classifying opinion texts at the document level or the sentence level is often insufficient for applications because they do not identify opinion targets or assign sentiments to such targets. Even if we assume that each document evaluates a single entity, a positive opinion document about the entity does not mean that the author has positive opinions about all aspects of the entity. Likewise, a negative opinion document does not mean that the author is negative about everything. For more complete analysis, we need to discover the aspects and determine whether the sentiment is positive or negative on each aspect.

At the aspect level, the objective is to discover every quintuple (ei, aij, sijkl, hk, tl) in a given document d. To achieve this goal, six tasks have to be performed.

### Aspect extraction

This task extracts aspects that have been evaluated. For example, in the sentence, “The voice quality of this phone is amazing," the aspect is “voice quality” of the entity represented by “this phone.” Note that “this phone” does not indicate the aspect GENERAL here because the evaluation is not about the phone as a whole, but only about its voice quality. However, the sentence “I love this phone” evaluates the phone as a whole, i.e., the GENERAL aspect of the entity represented by “this phone.” Bear in mind whenever we talk about an aspect, we must know which entity it belongs to. In our discussion below, we often omit the entity just for simplicity of presentation.

###  Aspect sentiment classification

This task determines whether the opinions on different aspects are positive, negative, or neutral. In the first example above, the opinion on the “voice quality” aspect is positive. In the second, the opinion on the aspect GENERAL is also positive.

There are two main approaches for determining the orientation of sentiment expressed on each aspect in a sentence: the supervised learning approach and the lexicon-based approach.

#### lexicon-based method

 - Mark sentiment words and phrases: For each sentence that contains one or more aspects, this step marks all sentiment words and phrases in the sentence. Each positive word is assigned the sentiment score of +1 and each negative word is assigned the sentiment score of 􏰉1. 
 - Apply sentiment shifters: Sentiment shifters (also called valence shifters in (Polanyi and Zaenen, 2004)) are words and phrases that can change sentiment orientations.
 - Handle but-clauses: Words or phrases that indicate contrary need special handling because they often change sentiment orientations too.
 - Aggregate opinions: This step applies an opinion aggregation function to the resulting sentiment scores to determine the final orientation of the sentiment on each aspect in the sentence.

### Basic Rules of Opinions and Compositional Semantics

An opinion rule expresses a concept that implies a positive or negative sentiment. It can be as simple as individual sentiment words with their implied sentiments or compound expressions that may need commonsense or domain knowledge to determine their orientations.

The rules are presented using a formalism similar to the BNF form. The rules are from (Liu, 2010).

![此处输入图片的描述][1]

1. Sentiment word or phrase

![此处输入图片的描述][2]

2. Decreased and increased quantity of an opinionated item (N and P)

![此处输入图片的描述][3]

3. High, low, increased and decreased quantity of a positive or negative potential item

![此处输入图片的描述][4]

4. Desirable or undesirable fact

![此处输入图片的描述][5]

5. Deviation from the norm or a desired value range

![此处输入图片的描述][6]

6. Produce and consume resource and waste

![此处输入图片的描述][7]

### Aspect Extraction

The key characteristic is that an opinion always has a target. The target is often the aspect or topic to be extracted from a sentence. Thus, it is important to recognize each opinion expression and its target from a sentence. However, we should also note that some opinion expressions can play two roles, i.e., indicating a positive or negative sentiment and implying an (implicit) aspect (target).

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
