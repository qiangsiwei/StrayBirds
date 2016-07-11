---
layout: post
title: Knowledge-bases and Crowdsourcing
category: Systems
comments: true
---

# KATARA: A Data Cleaning System Powered by Knowledge Bases and Crowdsourcing

------

Classical approaches to clean data have relied on using integrity constraints, statistics, or machine learning. These approaches are known to be limited in the cleaning accuracy, which can usually be improved by consulting master data and involving experts to resolve ambiguity. The advent of knowledge bases (kbs), both general-purpose and within enterprises, and crowdsourcing marketplaces are providing yet more opportunities to achieve higher accuracy at a larger scale.

Effectively exploring kbs and crowd in data cleaning raises several new challenges.

 - Matching (dirty) tables to kbs is a hard problem. Tables may lack reliable, comprehensible labels, thus requiring the matching to be executed on the data values. This may lead to ambiguity; more than one mapping may be possible.
 - kbs are usually incomplete in terms of the coverage of values in the table, making it hard to find correct table patterns and associate kb values.
 - Human involvement is needed to validate matchings and to verify data when the kbs do not have enough coverage.

Present Katara, the first data cleaning system that leverages prevalent trustworthy kbs and crowd-sourcing for data cleaning. Given a dirty table and a kb, Katara first discovers table patterns to map the table to the kb. With table patterns, Katara annotates tuples as either correct or incorrect by interleaving kbs and crowdsourcing. For incorrect tuples, Katara will extract top-k mappings from the kb as possible repairs.

## Overview

Katara consists of three modules pattern discovery, pattern validation, and data annotation. The pattern discovery module discovers table patterns between a table and a kb. The pattern validation module uses crowdsourcing to select one table pattern. Using the selected table pattern, the data annotation module interacts with the kb and the crowd to annotate data. It also generates possible repairs for erroneous tuples.

![此处输入图片的描述][1]

## Preliminaries

Table pattern. A table pattern (pattern for short) phi of a table T is a labelled directed graph G(V,E) with nodes V and edges E.

Semantics. A tuple t of T matches a table pattern phi containing m nodes {v1,..,vm} w.r.t. a kb K, denoted by t |= phi, if there exist m distinct attributes {A1,...,Am} in T and m resources {x1,...,xm} in K such that:

![此处输入图片的描述][2]

We say that a tuple t of T partially matches a table pattern phi w.r.t. K, if at least one of condition 2 and condition 3 holds.

Given a table T, a kb K, and a pattern phi, the following figure shows how Katara works on T. For non-covered attributes, could ask the crowd open-ended questions.

![此处输入图片的描述][3]

## Table Pattern Discovery

### Candidate Type/Relationship Discovery

Issue the following SPARQL query which returns the types and supertypes of entities whose label (i.e., value) is t[Ai].

![此处输入图片的描述][4]

Similarly, the relationship between two values t[Ai] and t[Aj] from a kb K can be retrieved via the two following SPARQL queries.

![此处输入图片的描述][5]

Query Q1rels retrieves relationships where the second attribute is a resource in kbs and Q2rels retrieves relationships where the second attribute is a literal value, i.e., untyped.

Use a normalized version of tf-idf (term frequency-inverse document frequency) to rank the candidate types of a column Ai.

![此处输入图片的描述][6]

### Scoring Model for Table Patterns

A table pattern contains types of attributes and properties between attributes. The space of all candidate patterns is very large (up to the Cartesian product of all possible types and relationships), making it expensive for human verification.

A naive scoring model for a candidate table pattern phi, consisting of type Ti for column Ai and relationship Pij for column pair Ai and Aj, is to simply add up all tf-idf scores of the candidate types and relationships in phi:

![此处输入图片的描述][7]

To quantify the "compatibility" between a type T and relationship P, where T serves as the type for the resources appearing as subjects of the relationship P, introduce a coherence score subSC(T,P). Similarly, to quantify the "compatibility" between a type T and relationship P, where T serves as the type for the entities appearing as objects of the relationship P, introduce a coherence scores objSC(T,P).

Use pointwise mutual information (PMI) as a proxy for computing subSC(T,P) and objSC(T,P).

![此处输入图片的描述][8]

![此处输入图片的描述][9]

![此处输入图片的描述][10]

Now define the score of a pattern phi as follows:

![此处输入图片的描述][11]

### Top-k Table Pattern Generation

Given a set of sorted lists and join conditions of those lists, the rank-join algorithm produces the top-k join results based on some score function for early termination without consuming all the inputs.

The algorithm, referred as PDiscovery. Given a table T, a kb K, and a number k, it produces top-k table patterns. To start, each input list, i.e., candidate types for a column, and candidate relationships for a column pair, is ordered according to the respective tf-idf scores. When two candidate types have the same tf-idf scores, the more discriminative type is ranked higher, i.e., the one with less number of instances in K.

![此处输入图片的描述][12]

![此处输入图片的描述][13]

## Pattern Validation via Crowd

Now study how to use the crowd to validate the discovered table patterns. Specifically, given a set P of candidate patterns, a table T, a kb K, and a crowdsourcing framework, need to identify the most appropriate pattern for T w.r.t. K, with the objective of minimizing the number of crowdsourcing questions.

### Creating Questions for the Crowd

A practical solution is to decompose table patterns into simple tasks:

 - type validation, i.e., to validate the type of a column in the table pattern.
 - binary relationship validation, i.e., to validate the relationship between two columns.

#### Column type validation

![此处输入图片的描述][14]

#### Relationship validation

![此处输入图片的描述][15]

### Question Scheduling

How to minimize the total number of questions to obtain the correct table pattern by scheduling which column and relationship to validate first.

Algorithm 3 describes the overall procedure for pattern validation.

 - choose the best variable vbest to validate next based on the expected reduction of uncertainty of phi.
 - remove from Pre those table patterns that have a different assignment for variable v than the validated value a.
 - renormalize the probability distribution of the remaining table patterns in Pre.
 - terminate when there are left with only one table pattern.

![此处输入图片的描述][16]

## Data Annotation

### Annotating Data

Katara annotates tuples as correct data validated by kbs, correct data jointly validated by kbs and the crowd, or data errors detected by the crowd, using the following two steps.

 - Step 1: Validation by kbs. For each tuple t and pattern phi, Katara issues a SPARQL query to check whether t is fully covered by a kb K. If it is fully covered, Katara annotates it as a correct tuple validated by kb (case (i)). Otherwise, it goes to step 2.
 - Step 2: Validation by kbs and Crowd. For each node (i.e., type) and edge (i.e., relationship) that is missing from K, Katara asks the crowd whether the relationship holds be- tween the given two values. If the crowd says yes, Katara annotates it as a correct tuple, jointly validated by kb and crowd (case (ii)). Otherwise, it is certain that there exist errors in this tuple (case (iii)).

### Generating Top-k Possible Repairs

A natural way to improve the naive solution for top-k possible repair generation is to retrieve only instance graphs that can possibly be repairs, i.e., the instance graphs whose values have an overlap with a given erroneous tuple.

The optimized algorithm is as follows. All possible repairs are initialized and instantiated by using inverted lists. For each possible repair, its repair cost w.r.t. t is computed, and top-k repairs are returned.

![此处输入图片的描述][17]

***相关连接***

 - https://cs.uwaterloo.ca/~ilyas/papers/ChuSIGMOD2015.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_11.png
  [12]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_12.png
  [13]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_13.png
  [14]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_14.png
  [15]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_15.png
  [16]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_16.png
  [17]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-13-knowledge_bases_and_crowdsourcing/2016-07-13-knowledge_bases_and_crowdsourcing_17.png

