---
layout: post
title: Knowledge-base Construction
category: Systems
comments: true
---

# DeepDive: Web-scale Knowledge-base Construction

------

知识库是用于知识管理的一种特殊的数据库，以便于有关领域知识的采集、整理以及提取。知识库中的知识源于领域专家，它是求解问题所需领域知识的集合，包括基本事实、规则和其它有关信息。

之前总结的知识图谱和知识库有一定的异同之处，知识图谱可视为知识库的一种图形化表达，更强调知识之间的拓扑关系。

DeepDive是Stanford开发的自动化知识库构建系统，主要通过统计学习、推理系统从多源数据中抽取知识。

DeepDive面临的主要挑战是数据的可扩展性，其一是数据量巨大，其二是数据中包含噪声。

DeepDive employs statistical learning and inference to combine diverse data resources and best-of-breed algorithms. A key challenge of this approach is scalability, i.e., how to deal with terabytes of imperfect data efficiently.

DeepDive起始于2012年1月，最初处理的语料库是英文网页数据ClueWeb09，之后经历了一系列扩充。

DeepDive went live in January 2012 after processing the 500M English web pages in the ClueWeb09 corpus2, and since then has been adding several million newly-crawled webpages every day.

DeepDive的使用可参考stanford主页，并开源于Github。

<http://deepdive.stanford.edu/>

<https://github.com/HazyResearch/deepdive>

知识库构建首先需要从文本中抽取实体和实体间关系，并经过一系列计算得出真理或断言（facts or assertions），目前已经有不少知识库构建系统。

Knowledge-base construction (KBC) is the process of populating a knowledge base (KB) with facts (or assertions) extracted from text. It has recently received tremendous interest from academia, e.g., CMU’s NELL and MPI’s YAGO, and from industry, e.g., IBM’s DeepQA and Microsoft’s EntityCube.

知识库构建系统所面临的共同挑战之一是数据的不完整性和冲突。DeepDive基于实体关系模型（Entity-Relationship model），并采用distant supervision和Markov logic language来计算真理或断言。

A crucial challenge that these systems face is coping with imperfect or conflicting information from multiple sources.

DeepDive is based on the classic Entity-Relationship (ER) model and employs popular techniques such as distant supervision and the Markov logic language to combine a variety of signals.

DeepDive的界面如下。

Screenshots of DeepDive showing facts about Barack Obama and provenance. (Left) Facts about Obama in DeepDive; (Top Right) Sentences mentioning the fact that Obama went to Harvard Law School; (Bottom Right) Text from a web page annotated with entity mentions.

![此处输入图片的描述][1]

相比其他的知识库构建系统，DeepDive的不同点体现在两个方面，其一是基于深度语言处理（deep natural language processing）来抽取实体和关系；其二是数据量在web-scale，统计和推理系统均在已有的方法上进行了一系列优化。

 - Unlike prior large-scale KBC systems, DeepDive performs deep natural language processing (NLP) to extract useful linguistic features such as named-entity mentions and dependency paths3 from terabytes of text.
 - DeepDive performs web-scale statistical learning and inference using classic data-management and optimization techniques.

DeepDive的整体框架如下。

Architecture of DeepDive. DeepDive takes as input diverse data resources, converts them into relational features, and then performs machine learning and statistical inference to construct a KB.

![此处输入图片的描述][2]

为了构建知识库系统，DeepDive首先从文本中抽取出实体和实体间关系，之后经过统计、推理，与已有知识库系统融合（可理解为宽展和去噪）形成自己的知识库体系。

To populate a knowledge base, DeepDive first converts diverse input data (e.g., raw corpora and ontologies) into relational features using standard NLP tools and custom code. These features are then used to train statistical models representing the correlations between linguistic patterns and target relations. Finally, DeepDive combines the trained statistical models with additional knowledge (e.g., domain knowledge) into a Markov logic program that is then used to transform the relational features (e.g., candidate entity mentions and linguistic patterns) into a knowledge base with entities, relationships, and their provenance.

## Challenges

下面描述技术挑战，主要体现在特征抽取和统计、推理的可扩展性（海量数据）。

### Scaling feature extraction

原本的Hadoop系统在特征抽取时存在如下瓶颈，这是由其架构设计所决定的。

The throughput of Hadoop is often limited by pathological data chunks that take very long to process or even crash (due to Hadoop’s no-task-left-behind failure model).

 - Hadoop’s all-or-nothing approach to failure handling hinders throughput.
 - The number of cluster machines for Hadoop is limited.

为了提升特征抽取速度，DeepDive采用了Condor infrastructure，可参考如下。

<http://research.cs.wisc.edu/pkilab/condor/>

Thanks to the Condor infrastructure, were able to finish feature extraction on ClueWeb09 within a week by opportunistically assigning jobs on hundreds of workstations and shared cluster machines using a best-effort failure model.

### Scalability with statistical learning

DeepDive采用distant supervision学习算法，根据已有的知识库系统（例如Freebase）采用启发式算法首先生成一部分训练数据（silver-standard examples），从而进行模型的学习，并不断扩充训练集。

Employ the distant supervision technique that automatically generates what are called silver-standard examples by heuristically aligning raw text with an existing knowledge base such as Freebase.

为了横向扩展学习算法，DeepDive采用了Bismarck system，其支持了RDBMS中的许多机器学习算法，可参考如下。

<http://www.cs.stanford.edu/people/chrismre/papers/bismarck-full.pdf>

To scale up the performance of machine learning, leveraged the Bismarck system that executes a wide variety of machine learning techniques inside an RDBMS.

DeepDive采用Markov logic进行统计推理，可参考如下。

<http://homes.cs.washington.edu/~pedrod/papers/pilp.pdf>

For statistical inference, DeepDive employs a popular statistical-inference framework called Markov logic.

为了横向扩展算法，DeepDive在已有的系统Alchemy上进行了优化。

Found that existing MLN systems such as Alchemy do not scale to the datasets. To cope, designed and implemented two novel approaches to MLN inference by leveraging datamanagement and optimization techniques.

## Implementation Details

下面描述关键技术细节，首先是特征抽取，DeepDive抽取的特征主要包括mention-level（文本位置、包含单词、正则匹配等）和entity-level（实体的属性如年龄、性别、别名等）。

DeepDive populates the target KB based on signals from mention-level features (over text spans, e.g., positions, contained words, and matched regular expressions) and entity-level features (over the target KB, e.g., age, gender, and alias).

### Entity Linking

实体连接，是指从文本数据到真实世界的映射，DeepDive主要借助了Wikipedia等外部知识库。

Entity linking is the task of mapping a textual mention to a real-world entity.

DeepDive then tries to map each mention to a Wikipedia entry using the following signals: string matching, Wikipedia redirects and inter-page anchor text, Google and Bing search results that link to Wikipedia pages, entity type compatibility between StanfordNER and Freebase, person-name coreference based on heuristics and proximity.

### Relation Extraction

关系抽取，DeepDive采用了MaltParser和Ensemble进行dependency parsing，并采用distant supervision基于Freebase等生成了大量可能的关系，采用sparse logistic regression从词汇和文法两方面特征对关系进行推断。

During feature extraction, perform dependency parsing using MaltParser and Ensemble.

Use Freebase and about 2M high-quality news and blog articles provided by TAC-KBP to perform distant supervision, generating about 1M training examples over 20 target relations. 

Use sparse logistic regression (l1 regularized) classifiers to train statistical relation-extraction models using both lexical (e.g., word sequences) and syntactic (e.g., dependency paths) features.

***相关连接***

 - http://research.cs.wisc.edu/machine-learning/shavlik-group/niu.vlds12.pdf
 - http://cs.stanford.edu/people/czhang/zhang.thesis.pdf
 - http://www.vldb.org/pvldb/vol8/p1310-shin.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-11-knowledge_base_construction/2016-07-11-knowledge_base_construction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-11-knowledge_base_construction/2016-07-11-knowledge_base_construction_2.png
