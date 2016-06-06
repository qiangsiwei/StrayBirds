---
layout: post
title: Relation Extraction Algorithms
category: Notes
comments: true
---

# Relation Extraction Algorithms

------

### Relation Extraction Algorithms

基于非结构化文本的实体关系抽取技术方法可以归纳为：基于模式匹配的关系抽取、基于词典驱动的关系抽取、基于机器学习的关系抽取、基于本体的关系抽取以及混合抽取方法。

其中基于机器学习的抽取方法可以粗分为Supervised和Semi-supervised，下面具体介绍各种算法。

## Feature based Methods(Supervised)

For the sake of simplicity and clarity, only restrict to binary relations between two entities. N-ary relation extraction will be discussed later.

Given a sentence S = w1,w2,..,e1..,wj,..e2,..,wn, where e1 and e2 are the entities, a mapping function f(.) can be given as

![此处输入图片的描述][1]

where T(S) are features that are extracted from S.

the function f(.) can be constructed as a discriminative classifier like Perceptron, Voted Perceptron or Support Vector Machines (SVMs). These classifiers can be trained using a set of features selected after performing textual analysis (like POS tagging, dependency parsing, etc) of the labeled sentences.

基于特征的方法所使用的句法特征通常包括一下几个方面：

 - the entities themselves
 - the types of the two entities
 - word sequence between the entities
 - number of words between the entities
 - path in the parse tree containing the two entities

Feature based methods involve heuristic choices and the features have to be selected on a trial-and-error basis in order to maximize performance.

## Bag of features Kernel(Supervised)

To remedy the problem of selecting a suitable feature-set, specialized kernels are designed for relation extraction in order to exploit rich representations of the input data like shallow parse trees etc.

The kernels used for relation-extraction (or relation-detection) are based on string-kernels described in (Lodhi et al., 2002). Given two strings x and y, the string-kernel computes their similarity based on the number of subsequences that are common to both of them. Each string can be mapped to a higher dimensional space where each dimension corresponds to the presence (weighted) or absence (0) of a particular subsequence.

![此处输入图片的描述][2]

If U is the set of all possible ordered subsequences present in strings x and y, the kernel similarity between x and y is given by

![此处输入图片的描述][3]

A straightforward computation of (4) using (3) involves exponential complexity. In practice however, the computation of equation (4) is performed implicitly in a efficient manner using a dynamic programming technique described in (Lodhi et al., 2002). The complexity of computation of the string-kernel given in (4) is O(|x||y|2) (|x| ∏ |y|).

<http://www.jmlr.org/papers/volume2/lodhi02a/lodhi02a.pdf>

A sentence s = w1, ..., e1, ..., wi, ..., e2, ..., wn containing related entities e1 and e2 can be described as s = sb e1 sm e2 sa. Here sb, sm and sa are portions of word-context before, middle and after the related entities respectively. Now given a test sentence t containing entities e01 and e02, the similarity of its before, middle and after portions with those of sentence s is computed using the sub-sequence kernel based on (4).

Bunescu et. al. use three subkernels, one each for matching the before, middle and after portions of the entities context and the combined kernel is simply the sum of all the subkernels and augment the words in the context with their respective part-of-speech (POS) tags, entity types etc to improve over the dependency tree kernel approach.

## Tree Kernels(Supervised)

Compared to the previous approach, (Zelenko et al., 2003) replace the strings in the kernel of equation (4) with structured shallow parse trees built on the sentence. 

The rationale for using shallow parse trees instead of full parse trees is that they tend to be more robust and reliable.

Now given a shallow parse, positive examples are constructed by enumerating the lowest subtrees which cover both the related entities. If a subtree does not cover the related entities, it is assigned a negative label. Given two shallow parse trees the kernel computes the structural commonality between them.

![此处输入图片的描述][4]

The kernel similarity is computed recursively as follows:

![此处输入图片的描述][5]

The shortest path between the two entities in a dependency parse encodes sufficient information to extract the relation between them. If e1 and e2 are two entities in a sentence and p their predicate, then the shortest path between e1 and e2 passes through p. Let one such path be P = e1 → w1  .. → wi ← .. ← wn ← e2. 

Using P alone as a feature would lead to bad performance due to data sparsity. Therefore word-classes like POS are extracted from each of the words and the feature vectors is given by a Cartesian product as

![此处输入图片的描述][6]

## DIPRE(Semi-supervised)

The main idea of both algorithms is to use the output of the weak learners as training data for the next iteration.

To ensure provable performance guarantees, the co-training algorithm assumes as input a set of views that satisfies two fairly strict conditions. First, each view must be sufficient for learning the target concept. Second, the views must be conditionally independent of each other given the class.

![此处输入图片的描述][7]

The general framework is shown in Algorithm 1. The system crawls the Internet to look for pages containing both instances of the seed. To learn patterns DIPRE uses a tuple of 6 elements [order,author,book,prefix,su±x,middle]. The next step is to generalize the pattern with a wild card expression. DIPRE uses this pattern to search the Web again, and extract relations.

## Snowball(Semi-supervised)

Snowball represents each tuple as a vector and uses a similarity function to group tuples.

![此处输入图片的描述][8]

Each wi is a term weight which is computed by the normalized frequency of that term in a given position. For example, term weight of token meet in the suffix is

![此处输入图片的描述][9]

To group tuples which have the same (organization, location) representation but differ in prefix, suffix, and middle Snowball introduce the similarity function

![此处输入图片的描述][10]

After grouping tuples into classes, Snowball induces a single tuple pattern P for each class which is a centroid vector tuple. Each pattern P is assigned a confidence score which measure the quality of a newly-proposed pattern

![此处输入图片的描述][11]

Compared to DIPRE, Snowball has a flexible matching system. Instead of having exact surface text matches, Snowball’s metrics allows for slight variations in token or punctuation.

## KnowItAll and TextRunner(Semi-supervised)

Unlike DIPRE and Snowball, KnowItAll is a large scale Web IE system that labels its own training examples using only a small set of domain independent extraction patterns.

The rules are applied to web pages, identified via search-engine queries, and the resulting extractions are assigned a probability using pointwise mutual information (PMI) derived from search engine hit counts.

Examples: "<NP1> such as <NP2>" & "capital of <country>"

DIPRE, Snowball, and KnowItAll are all relation-specific systems. Instead of requiring relations to be specified in its input, TextRunner learns the relations, classes, and entities from the text in its corpus in a self-supervised fashion.

![此处输入图片的描述][12]

The Learner first automatically labeled its own training data as positive or negative, then it uses this labeled data to train a binary classifier which is used by Extractor. The Extractor generates one or more candidate relations from each sentence, then runs a classifier and retains the ones labeled as trustworthy relations. The Assesor assigns a probability to each retained tuple based on a probabilistic model of redundancy.

Parsers are only used once when the Learner performs its self-training process for the classifier. The key idea of the system is the Extractor extracts relations from a large amount of text without running the dependency parser.

## Beyond Binary Relations

For example, we are interested in the ternary relation (organizer, conference, location) that relates an organizer to a conference at a particular location. For a sentence “ACL-2010 will be hosted by CMU in Pittsburgh”, the system should extract (CMU, ACL-2010, Pittsburgh).

 - Start by recognizing binary relation instances that appear to be arguments of the relation of interest.
 - Extractedbinaryrelationscanbetreatedastheedgesofgraphwithentitymentionasnodes.
 - Reconstruct complex relations by making tuples from selected maximal cliques in the graph.

## Evaluating Relation-Extraction

Datasets available for relation extraction evaluation.

![此处输入图片的描述][13]

Evaluation in Supervised Methods

![此处输入图片的描述][14]

Evaluation of Semi-supervised Methods

A small sample drawn randomly from the output is treated as a representative of the output and manually checked for actual relations. Since the actual number of entity relations are difficult to obtain from large amounts of data, it is difficult to compute recall to evaluate semi-supervised methods.

***相关连接***

 - http://www.cs.cmu.edu/~nbach/papers/A-survey-on-Relation-Extraction.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_11.png
  [12]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_12.png
  [13]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_13.png
  [14]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-06-relation_extraction_algorithms/2016-06-06-relation_extraction_algorithms_14.png
