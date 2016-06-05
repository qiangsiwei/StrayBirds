---
layout: post
title: Knowledge Graph
category: Notes
comments: true
---

# Knowledge Graph

------

### Knowledge Graph

The first is based on latent feature models such as tensor factorization and multiway neural networks. The second is based on mining observable patterns in the graph.

In Statistical Relational Learning (SRL), the representation of an object can contain its relationships to other objects. Thus the data is in the form of a graph, consisting of nodes (entities) and labelled edges (relationships between entities). The main goals of SRL include prediction of missing edges, prediction of properties of nodes, and clustering nodes based on their connectivity patterns.

Recently, a large number of knowledge graphs have been created, including YAGO, DBpedia, NELL, Freebase, and the Google Knowledge Graph.

![此处输入图片的描述][1]

Sample knowledge graph. Nodes represent entities, edge labels represent types of relations, edges represent existing relationships.

![此处输入图片的描述][2]

Represent facts in the form of binary relationships, in particular (subject, predicate, object) (SPO) triples, where subject and object are entities and predicate is the relation between them.

![此处输入图片的描述][3]

Combine all the SPO triples together to form a multi- graph, where nodes represent entities (all subjects and objects), and directed edges represent relationships.

Different paradigms for the interpretation of non-existing triples:

 - Under the closed world assumption (CWA), non-existing triples indicate false relationships. 
 - Under the open world assumption (OWA), a non-existing triple is interpreted as unknown, i.e., the corresponding relationship can be either true or false.

Completeness, accuracy, and data quality are important parameters that determine the usefulness of knowledge bases and are influenced by the way knowledge bases are constructed.

Knowledge base construction methods:

 - In curated approaches, triples are created manually by a closed group of experts.
 - In collaborative approaches, triples are created manually by an open group of volunteers.
 - In automated semi-structured approaches, triples are extracted automatically from semi-structured text (e.g., infoboxes in Wikipedia) via hand-crafted rules, learned rules, or regular expressions.
 - In automated unstructured approaches, triples are extracted automatically from unstructured text via machine learning and natural language processing techniques.

### LATENT FEATURE MODELS

Latent feature models capture the correlation between the nodes/edges using latent variables. The key intuition behind relational latent feature models is that the relationships between entities can be derived from interactions of their latent features.

RESCAL: A bilinear model

RESCAL is a relational latent feature model which explains triples via pairwise interactions of latent features.

![此处输入图片的描述][4]

where Wk is a weight matrix whose entries wabk specify how much the latent features a and b interact in the k-th relation. It is called a bilinear model, since it captures the interactions between the two entity vectors using multiplicative terms.

![此处输入图片的描述][5]

![此处输入图片的描述][6]

Since all parameters are learned jointly, these shared representations permit to propagate information between triples via the latent representations of entities and the weights of relations. This allows the model to capture global dependencies in the data.

Other Methods:

 - Other tensor factorization models
 - Matrix factorization methods
 - Multi-layer perceptrons
 - Neural tensor networks
 - Latent distance models

![此处输入图片的描述][7]

### GRAPH FEATURE MODELS

Graph feature models capture the correlation directly using statistical models based on the observable properties of the graph. In graph feature models, it is assumed that the existence of an edge can be predicted by extracting features from the observed edges in the graph.

1. Similarity measures for uni-relational data   
The intuition behind these methods is that similar entities are likely to be related (homophily) and that the similarity of entities can be derived from the neighborhood of nodes or from the existence of paths between nodes. For this purpose, various indices have been proposed to measure the similarity of entities, which can be classified into local, global, and quasi-local approaches.

2. Rule Mining and Inductive Logic Programming   
Another class of models that works on the observed variables of a knowledge graph extracts rules via mining methods and uses these extracted rules to infer new links. The extracted rules can also be used as a basis for Markov Logic.

3. Path Ranking Algorithm
The Path Ranking Algorithm (PRA) extends the idea of using random walks of bounded lengths for predicting links in multi-relational knowledge graphs.

***相关连接***

 - https://arxiv.org/pdf/1503.00759.pdf
 - http://people.ee.duke.edu/~lcarin/Piyush06.12.2015.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-03-knowledge_graph/2016-06-03-knowledge_graph_7.png

  