---
layout: post
title: Relation Extraction
category: Notes
comments: true
---

# Relation Extraction

------

### Relation Extraction

Normally, we are interested in relations between entities, such as person, organization, and location. Examples of relations are person-affiliation and organization- location. Current state-of-the-art named entities recognizers (NER), such as (Bikel et al., 1999) and (Finkel et al., 2005), can automatically label data with high accuracy.

A relation is defined in the form of a tuple t = (e1,e2,...,en) where the ei are entities in a pre- defined relation r within document D. Most relation extraction systems focus on extracting binary relations. Examples of binary relations include located-in(CMU, Pittsburgh), father-of(Manuel Blum, Avrim Blum). It is also possible to go to higher-order relations as well.

### Supervised Methods

The task of relation extraction is presented as a classification task.   
Given a sentence S = w1,w2,..,e1..,wj,..e2,..,wn, where e1 and e2 are the entities, a mapping function f (.) can be given as

![此处输入图片的描述][1]

The function f (.) can be constructed as a discriminative classifier like Perceptron, Voted Perceptron or Support Vector Machines (SVMs).

特征来自于POS tagging，dependency parsing等

可以进一步细分成：(1) feature based methods 和 (2) kernel methods。

句法特征通常包括：

 - the entities themselves
 - the types of the two entities
 - word sequence between the entities
 - number of words between the entities
 - path in the parse tree containing the two entities

The major advantage of kernel methods is they offer efficient solutions that allow us to explore a large (often exponential, or in some cases, infinite) feature space in polynominal computational time, without the need to explicitly represent the features.

For example, a string x = cat can be expressed in a higher dimensional space of subsequences as follows:

![此处输入图片的描述][2]

kernel similarity

![此处输入图片的描述][3]

1. Bag of features Kernel

(Bunescu & Mooney, 2005b) use the word-context around the named entities for extracting protein interactions from the MEDLINE abstracts. A sentence s = w1, ..., e1, ..., wi, ..., e2, ..., wn contain- ing related entities e1 and e2 can be described as s = sb e1 sm e2 sa. Here sb, sm and sa are portions of word-context before, middle and after the related entities respectively.

2. Tree Kernels

Compared to the previous approach, (Zelenko et al., 2003) replace the strings in the kernel with structured shallow parse trees built on the sentence.

Now given a shallow parse, positive examples are constructed by enumerating the lowest subtrees which cover both the related entities. If a subtree does not cover the related entities, it is assigned a negative label.

![此处输入图片的描述][4]

目前，经测试，基于dependency-path kernels的方法是所有基于kernel的方法中效果最好的。

Limitations:

 - These methods are difficult to extend to new entity-relation types for want of labeled data.
 - Extensions to higher order entity relations are difficult as well.
 - They are relatively computationally burdensome and do not scale well with increasing amounts of input data.
 - Most of the methods described require pre-processed input data in the form of parse tree, dependency parse trees etc. The pre-processing stage is error prone and can hinder the performance of the system.

### Semi-supervised Methods

Only require a small set of tagged seed instances or a few hand-crafted extraction patterns per relation to launch the training process.

Two primary questions:

 - How to obtain seed automatically?
 - What is a good seed?

1. DIPRE

The system crawls the Internet to look for pages containing both instances of the seed. To learn patterns DIPRE uses a tuple of 6 elements [order,author,book,prefix,suffix,middle].

![此处输入图片的描述][5]

2. Snowball

The classifier in Snowball is a pattern matching system like DIPRE, however, Snowball does not use exact matching. Snowball represents each tuple as a vector and uses a similarity function to group tuples. Snowball extracts tuples in form [prefix,orgnization,middle,location,suffix].

The limit on the prefix and suffix feature vectors, Each wi is a term weight which is computed by the normalized frequency of that term in a given position. For example, term weight of token meet in the suffix is

![此处输入图片的描述][6]

To group tuples which have the same (organization, location) representation but differ in prefix, suffix, and middle Snowball introduce the similarity function

![此处输入图片的描述][7]

After grouping tuples into classes, Snowball induces a single tuple pattern P for each class which is a centroid vector tuple. Each pattern P is assigned a confidence score which measure the quality of a newly-proposed pattern

![此处输入图片的描述][8]

3. KnowItAll and TextRunner

Unlike DIPRE and Snowball, KnowItAll (Etzioni et al., 2005) is a large scale Web IE system that labels its own training examples using only a small set of domain independent extraction patterns.

The system contains three components which are 1) self-supervised Learner ; 2) single-pass Extractor; and 3) redundancy-based Assesor.

![此处输入图片的描述][9]

Limitation:

A major limitation of all systems studied here is that they are extracting relation on the sentence level. In fact, relations can span over sentences and even cross documents. It is not straightforward to modify algorithms described here to capture long range relations.

***相关连接***

 - http://www.cs.cmu.edu/~nbach/papers/A-survey-on-Relation-Extraction.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-01-relation_extraction/2016-06-01-relation_extraction_9.png
