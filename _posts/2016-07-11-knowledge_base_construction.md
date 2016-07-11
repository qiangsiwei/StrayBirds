---
layout: post
title: Knowledge-base Construction
category: Papers
comments: true
---

# DeepDive: Web-scale Knowledge-base Construction

------

DeepDive employs statistical learning and inference to combine diverse data resources and best-of-breed algorithms. A key challenge of this approach is scalability, i.e., how to deal with terabytes of imperfect data efficiently.

DeepDive went live in January 2012 after processing the 500M English web pages in the ClueWeb09 corpus2, and since then has been adding several million newly-crawled webpages every day.

<http://deepdive.stanford.edu/>

Knowledge-base construction (KBC) is the process of populating a knowledge base (KB) with facts (or assertions) extracted from text. It has recently received tremendous interest from academia, e.g., CMU’s NELL and MPI’s YAGO, and from industry, e.g., IBM’s DeepQA and Microsoft’s EntityCube.

A crucial challenge that these systems face is coping with imperfect or conflicting information from multiple sources.

DeepDive is based on the classic Entity-Relationship (ER) model and employs popular techniques such as distant supervision and the Markov logic language to combine a variety of signals.

Screenshots of DeepDive showing facts about Barack Obama and provenance. (Left) Facts about Obama in DeepDive; (Top Right) Sentences mentioning the fact that Obama went to Harvard Law School; (Bottom Right) Text from a web page annotated with entity mentions.

![此处输入图片的描述][1]

DeepDive goes deeper in two ways: 

 - Unlike prior large-scale KBC systems, DeepDive performs deep natural language processing (NLP) to extract useful linguistic features such as named-entity mentions and dependency paths3 from terabytes of text.
 - DeepDive performs web-scale statistical learning and inference using classic data-management and optimization techniques.

Architecture of DeepDive. DeepDive takes as input diverse data resources, converts them into relational features, and then performs machine learning and statistical inference to construct a KB.

![此处输入图片的描述][2]

To populate a knowledge base, DeepDive first converts diverse input data (e.g., raw corpora and ontologies) into relational features using standard NLP tools and custom code. These features are then used to train statistical models representing the correlations between linguistic patterns and target relations. Finally, DeepDive combines the trained statistical models with additional knowledge (e.g., domain knowledge) into a Markov logic program that is then used to transform the relational features (e.g., candidate entity mentions and linguistic patterns) into a knowledge base with entities, relationships, and their provenance.

## Challenges

### Scaling feature extraction

The throughput of Hadoop is often limited by pathological data chunks that take very long to process or even crash (due to Hadoop’s no-task-left-behind failure model).

 - Hadoop’s all-or-nothing approach to failure handling hinders throughput.
 - The number of cluster machines for Hadoop is limited.

Thanks to the Condor infrastructure, were able to finish feature extraction on ClueWeb09 within a week by opportunistically assigning jobs on hundreds of workstations and shared cluster machines using a best-effort failure model.

### Scalability with statistical learning

Employ the distant supervision technique that automatically generates what are called silver-standard examples by heuristically aligning raw text with an existing knowledge base such as Freebase.

To scale up the performance of machine learning, leveraged the Bismarck system that executes a wide variety of machine learn- ing techniques inside an RDBMS.

For statistical inference, DeepDive employs a popular statistical-inference frame- work called Markov logic.

Found that existing MLN systems such as Alchemy do not scale to the datasets. To cope, designed and implemented two novel approaches to MLN inference by leveraging data- management and optimization techniques.

## Implementation Details

DeepDive populates the target KB based on signals from mention-level features (over text spans, e.g., positions, contained words, and matched regular expressions) and entity-level features (over the target KB, e.g., age, gender, and alias).

### Entity Linking

Entity linking is the task of mapping a textual mention to a real-world entity.

DeepDive then tries to map each mention to a Wikipedia entry using the following signals: string matching, Wikipedia redirects and inter-page anchor text, Google and Bing search results that link to Wikipedia pages, entity type compatibility between StanfordNER and Freebase, person-name coreference based on heuristics and proximity.

### Relation Extraction

During feature extraction, perform dependency parsing using MaltParser and Ensemble.

Use Freebase and about 2M high-quality news and blog articles provided by TAC-KBP to perform distant supervision, generating about 1M training examples over 20 target relations. 

Use sparse logistic regression (l1 regularized) classifiers to train statistical relation-extraction models using both lex- ical (e.g., word sequences) and syntactic (e.g., dependency paths) features.

***相关连接***

 - http://research.cs.wisc.edu/machine-learning/shavlik-group/niu.vlds12.pdf
 - http://cs.stanford.edu/people/czhang/zhang.thesis.pdf
 - http://www.vldb.org/pvldb/vol8/p1310-shin.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-11-knowledge_base_construction/2016-07-11-knowledge_base_construction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-11-knowledge_base_construction/2016-07-11-knowledge_base_construction_2.png
