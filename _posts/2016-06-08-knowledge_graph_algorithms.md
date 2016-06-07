---
layout: post
title: Knowledge Graph Algorithms
category: Notes
comments: true
---

# Knowledge Graph Algorithms

------

### Knowledge Graph Algorithms

知识图谱，也称为科学知识图谱，它通过将应用数学、图形学、信息可视化技术、信息科学等学科的理论与方法与计量学引文分析、共现分析等方法结合，用可视化技术描述知识资源及其载体，挖掘、分析、构建、绘制和显示知识及它们之间的相互联系。为学科研究提供切实的、有价值的参考。

知识图谱的构建通常可以粗分为下面4中方法。

 - In curated approaches, triples are created manually by a closed group of experts.
 - In collaborative approaches, triples are created manually by an open group of volunteers.
 - In automated semi-structured approaches, triples are extracted automatically from semi-structured text (e.g., infoboxes in Wikipedia) via hand-crafted rules, learned rules, or regular expressions.
 - In automated unstructured approaches, triples are extracted automatically from unstructured text via machine learning and natural language processing techniques.

目前知识图谱的成功案例及算法分类如下。

![此处输入图片的描述][1]

## Statistical relational learning for knowledge graphs

We model each possible triple xijk = (ei,rk,ej) over this set of entities and relations as a binary random variable yijk that indicates its existence.

![此处输入图片的描述][2]

Tensor representation of binary relational data is as follows.

![此处输入图片的描述][3]

An important issue for SRL on knowledge graphs is how to deal with the large number of possible relationships while efficiently exploiting the sparsity of relationships.

Knowledge graphs typically adhere to some deterministic rules, such as type constraints and transitivity and have typically also various "softer" statistical patterns or regularities, which are not universally true but nevertheless have useful predictive power.

 - Homophily: the tendency of entities to be related to other entities with similar characteristics.
 - Block structure: entities can be divided into distinct groups (blocks), such that all the members of a group have similar relationships to members of other groups.
 - Global and long-range statistical dependencies: dependencies that can span over chains of triples and involve different types of relations.

There are three types of SRL models:

 - Assume all yijk are conditionally independent given latent features associated with subject, object and relation type and additional parameters (latent feature models)
 - Assume all yijk are conditionally independent given observed graph features and additional parameters (graph feature models)
 - Assume all yijk have local interactions (Markov Random Fields)

The conditional independence assumptions of M1 and M2 allow the probability model to be written as follows:

![此处输入图片的描述][4]

\sigma is the sigmoid (logistic) function,

![此处输入图片的描述][5]

\Ber is the Bernoulli distribution.

![此处输入图片的描述][6]

In addition to probabilistic models, f(.) can be optimized under other criteria, for instance models which maximize the margin between existing and non-existing triples, which is refer as score-based models.

## Latent Feature Models

See the following table for a summary of the notation.

![此处输入图片的描述][7]

### RESCAL: A bilinear model

RESCAL is a relational latent feature model which explains triples via pairwise interactions of latent features. In particular, we model the score of a triple xijk as

![此处输入图片的描述][8]

RESCAL is called a bilinear model, since it captures the interactions between the two entity vectors using multiplicative terms.

For instance, we could model that Alec Guinness is a good actor and that the Academy Award is a prestigious award via the vectors

![此处输入图片的描述][9]

we could model the pattern that good actors are likely to receive prestigious awards via a weight matrix such as

![此处输入图片的描述][10]

The parameters of RESCAL can be estimated by minimizing the log-loss using gradient-based methods such as stochastic gradient descent. 

A single update of E and Wk scales linearly with the number of entities Ne, linearly with the number of relations Nr, and linearly with the number of observed triples, i.e., the number of non-zero entries in Y, it is called algorithm RESCAL-ALS.

### Other tensor factorization models

Various other tensor factorization methods have been explored for learning from knowledge graphs and multi-relational data, including factorizing adjacency tensors using the CP tensor decomposition to analyze the link structure of Web pages and Semantic Web data respectively.

### Matrix factorization methods

The adjacency tensor is reshaped into a matrix by associating rows with subject-object pairs and columns with relations, or into a matrix by associating rows with subjects and columns with relation/objects. Unfortunately, both of these formulations lose information compared to tensor factorization.

### Multi-layer perceptrons

In particular, we can rewrite RESCAL as

![此处输入图片的描述][11]

Multi-layer perceptrons allows us to consider alternative ways to create composite
triple representations and to use nonlinear functions to predict their existence.

In particular, E-MLP model is defined as follows

![此处输入图片的描述][12]

One disadvantage of the E-MLP is that it has to define a vector wk and a matrix Ak for every possible relation, and ER-MLP is defined as follows, C is independent of the relation k.

![此处输入图片的描述][13]

Visualization of RESCAL and the ER-MLP model as Neural Networks is as follows. Here, He = Hr = 3 and Ha = 3. Note, that the inputs are latent features.

![此处输入图片的描述][14]

### Neural tensor networks

We can combine traditional MLPs with bilinear models, resulting in what calls a "neural tensor network" (NTN). More precisely, we can define the NTN model as follows:

![此处输入图片的描述][15]

### Latent distance models

Latent distance models derive the probability of relationships from the distance between latent representations of entities: entities are likely to be in a relationship if their latent representations are close according to some distance measure.

The structured embedding (SE) model extends this idea to multi-relational data by modeling the score of a triple xijk as:

![此处输入图片的描述][16]

To reduce the number of parameters over the SE model, the TransE model translates the latent feature representations via a relation-specific offset instead of transforming them via matrix multiplications.

![此处输入图片的描述][17]

Following table summarizes the different models we have discussed. Clearly the best model will be dataset dependent.

![此处输入图片的描述][18]

## Graph Feature Models

In graph feature models, we assume that the existence of an edge can be predicted by extracting features from the observed edges in the graph.

### Similarity measures for uni-relational data

The intuition behind these methods is that similar entities are likely to be related (homophily) and that the similarity of entities can be derived from the neighborhood of nodes or from the existence of paths between nodes.

 - Local similarity indices such as Common Neighbors, the Adamic-Adar index or Preferential Attachment derive the similarity of entities from their number of common neighbors or their absolute number of neighbors. However, they can be too localized to capture important patterns in relational data and cannot model long-range or global dependencies.
 - Global similarity indices such as the Katz index and the Leicht-Holme-Newman index derive the similarity of entities from the ensemble of all paths between entities, while indices like Hitting Time, Commute Time, and PageRank derive the similarity of entities from random walks on the graph.
 - Quasi-local similarity indices like the Local Katz index or Local Random Walks try to balance predictive accuracy and computational complexity by deriving the similarity of entities from paths and random walks of bounded length.

### Rule Mining and Inductive Logic Programming

This method extracts rules via mining methods and uses these extracted rules to infer new links.

 - ALEPH is an Inductive Logic Programming (ILP) system that attempts to learn rules from relational data via inverse entailment.
 - AMIE is a rule mining system that extracts logical rules based on their support in a knowledge graph.

Rule-based method is easily interpretable. However, rules over observed variables cover usually only a subset of patterns in knowledge graphs (or relational data) and useful rules can be challenging to learn.

### Path Ranking Algorithm

We can compute the probability of following a path by assuming that at each step, we follow an outgoing link uniformly at random. The key idea in PRA is to use these path probabilities as features for predicting the probability of missing edges.

![此处输入图片的描述][19]

We can then predict the edge probabilities using logistic regression:

![此处输入图片的描述][20]

## Combining latent and graph feature models

The strengths of latent and graph-based models are often complementary, as both families focus on different aspects of relational data:

 - Latent feature models are well-suited for modeling global relational patterns via newly introduced latent variables. They are computationally efficient if triples can be explained with a small number of latent variables.
 - Graph feature models are well-suited for modeling local and quasi-local graphs patterns. They are computationally efficient if triples can be explained from the neighborhood of entities or from short paths in the graph.

### Additive relational effects model

![此处输入图片的描述][21]

ARE models can be trained by alternately optimizing the RESCAL parameters with the PRA parameters.

### Other combined models

![此处输入图片的描述][22]

The term \Phi captures patterns efficiently where the existence of a triple y' is predictive of another triple y between the same pair of entities (but of a different relation type).

An alternative way to combine different prediction systems is to fit them separately, and use their outputs as inputs to another "fusion" system. 

Stacking has the advantage that it is very flexible in the kinds of models that can be combined. However, it has the disadvantage that the individual models cannot cooperate, and thus any individual model needs to be more complex than in a combined model which is trained jointly.

***相关连接***

 - https://arxiv.org/pdf/1503.00759.pdf
 - http://people.ee.duke.edu/~lcarin/Piyush06.12.2015.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_7.png
  [8]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_8.png
  [9]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_9.png
  [10]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_10.png
  [11]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_11.png
  [12]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_12.png
  [13]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_13.png
  [14]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_14.png
  [15]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_15.png
  [16]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_16.png
  [17]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_17.png
  [18]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_18.png
  [19]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_19.png
  [20]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_20.png
  [21]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_21.png
  [22]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-08-knowledge_graph_algorithms/2016-06-08-knowledge_graph_algorithms_22.png
