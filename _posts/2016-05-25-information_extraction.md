---
layout: post
title: Information Extraction
category: Notes
comments: true
---

# Information Extraction

------

### Information Extraction


Open Information Extraction (IE) is the task of extracting assertions from massive corpora without requiring a pre-specified vocabulary.

Conventionally, open IE systems search a collection of patterns over either the surface form or dependency tree of a sentence.

Several Open IE systems have been proposed before now, including TEXTRUNNER [Banko et al., 2007], WOE [Wu and Weld, 2010], and StatSnowBall [Zhu et al., 2009]. All these systems use the following three-step method:

 - Label: Sentences are automatically labeled with extractions using heuristics or distant supervision.
 - Learn: A relation phrase extractor is learned using a sequence-labeling graphical model (e.g., CRF).
 - Extract: the system takes a sentence as input, identifies a candidate pair of NP arguments (Arg1, Arg2) from the sentence, and then uses the learned extractor to label each word between the two arguments as part of the relation phrase or not.

Generating coherent clauses before applying patterns helps reduce false matches such as (Honolulu; be born in; Hawaii).

Coherent clauses which are (1) logically entailed by the original sentence, and (2) easy to segment into open IE triples.

First learn a classifier for splitting a sentence into shorter utterances, and then appeal to natural logic to maximally shorten these utterances while maintaining necessary context. A small set of 14 hand-crafted patterns can then be used to segment an utterance into an open IE triple.

Treat the first stage as a greedy search problem: traverse a dependency parse tree recursively, at each step predicting whether an edge should yield an independent clause. The objective is to produce a set of clauses which can stand on their own syntactically and semantically, and are entailed by the original sentence.

![此处输入图片的描述][1]

From left to right, a sentence yields a number of independent clauses (e.g., she Born in a small town). From top to bottom, each clause produces a set of entailed shorter utterances, and segments the ones which match an atomic pattern into a relation triple

Train a multinomial logistic regression classifier on our noisy training data.

The feature class is a high level description of features; the feature templates are the particular templates used. For instance, the POS signature contains the tag of the parent, the tag of the child, and both tags joined in a single feature. Note that all features are joined with the action to be taken on the parent.

![此处输入图片的描述][2]

Two significant problems in all prior Open IE systems: incoherent extractions and uninformative extractions. Incoherent extractions are cases where the extracted relation phrase has no meaningful interpretation. Uninformative extractions, occurs when extractions omit critical information.

![此处输入图片的描述][3]

Two simple constraints: Syntactic Constraint and Lexical Constraint.

A simple part-of-speech-based regular expression reduces the number of incoherent extractions. If the pattern matches multiple adjacent sequences, they are merged into a single relation phrase.

![此处输入图片的描述][4]

ARGLEARNER divides this task into two subtasks - finding Arg1 and Arg2 - and then subdivides each of these subtasks again into identifying the left bound and the right bound of each argument. ARGLEARNER employs three classifiers to this aim. Two classifiers identify the left and right bounds for Arg1 and the last classifier identifies the right bound of Arg2.

![此处输入图片的描述][5]

The goal is to find the largest subset of language from which we can extract reliably and efficiently. Observations for frequent argument cat- egories, both for Arg1 and Arg2.

![此处输入图片的描述][6]


***相关连接***

 - https://web.stanford.edu/~angeli/papers/2015-acl-openie.pdf
 - https://homes.cs.washington.edu/~soderlan/Fader-emnlp11.pdf
 - http://turing.cs.washington.edu/papers/etzioni-ijcai2011.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_4.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_5.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-25-information_extraction/2016-05-25-information_extraction_6.png
