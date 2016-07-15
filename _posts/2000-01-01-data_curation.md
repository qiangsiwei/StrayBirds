---
layout: post
title: Data Curation
category: Systems
comments: true
---

# Data Curation at Scale: The Data Tamer System

------

Data curation is the act of discovering a data source(s) of interest, cleaning and transforming the new data, semantically integrating it with other local data sources, and deduplicating the resulting composite.

For example, one web aggregator requires the curation of 80,000 URLs and a second biotech company has the problem of curating 8000 spreadsheets. At this scale, data curation cannot be a manual (human) effort, but must entail machine learning approaches with a human assist only when necessary.

This paper describes Data Tamer, an end-to-end curation system we have built at M.I.T. Brandeis, and Qatar Computing Research Institute (QCRI).

<https://cs.uwaterloo.ca/~ilyas/papers/StonebrakerCIDR2013.pdf>

We have run Data Tamer on three real world enterprise curation problems, and it has been shown to lower curation cost by about 90%, relative to the currently deployed production software.

## Characteristics

These four issues should be addressed in a single coherent architecture, which we call a data curation system.

 - Scalability through automation. The size of the integration problems we are encountering precludes a human-centric solution. Next generation systems will have to move to automated algorithms with human help only when necessary. In addition, advances in machine learning and the application of statistical techniques can be used to make many of the easier decisions automatically.
 - Data cleaning. Enterprise data sources are inevitably quite dirty. Attribute data may be incorrect, inaccurate or missing. Again, the scale of future problems requires an automated solution with human help only when necessary.
 - Non-programmer orientation. Current Extract, Transform and Load (ETL) systems have scripting languages that are appropriate for professional programmers. The scale of next generation problems requires that less skilled employees be able to perform integration tasks.
 - Incremental. New data sources must be integrated incrementally as they are uncovered. There is never a notion of the integration task being finished.

## Data Tamer Semantic Model

### Human Roles

A Data Tamer administrator (DTA). This role is analogous to a traditional database administrator.

One or more Domain Experts (DE). These are human domain experts that can be called on to answer questions that arise during the curation process.

### Sites and Schemas

Level 3: Complete knowledge. In this case, the complete global schema for a given class of entities has been specified by the DTA using a top-down methodology.

Level 1: No knowledge available. In this case, nothing is known about the structure of the classes of information, and a bottom-up integration is utilized.

Level 2: Partial information available. Using either a top-down or bottom-up methodology, there may be partial information available. There may be specific attributes that are known to be present for some class of entities. Alternately, there may be templates available. A template is a collection of attributes that are likely to appear together in one of more classes of entities.

### Other Information

In addition, in many domains, there are standard dictionaries, which should be used by Data Tamer. A dictionary is a list of data values of some data type that can populate an attribute in some data source.

### Management Console and Data Tamer Actions

Sites, categories, templates, dictionaries, authoritative tables, and synonyms can be specified through a DTA management console, which is a fairly traditional-looking GUI.

A portion of this console is dedicated to allowing the DTA to specify actions for Data Tamer to take. These actions are:

 - Ingest a new data source, and store the incoming data in a Postgres database.
 - Perform attribute identification on data source-i.
 - Perform entity consolidation on data source-i.

At any time during attribute identification or entity consolidation, Data Tamer may request human assistance from a DE.

At any time, the DTA may request that attribute identification and/or entity consolidation be redone on all sites.

Lastly, Data Tamer keeps a history of all operations performed, and the DTA may "snap" the curation process backwards to any past historical point.

### Training Data

The first one is appropriate for cases with minimal or no advance knowledge (i.e., levels 1 and 2 above). As such, training data is simply accumulated over time by the crowd-sourcing component of Data Tamer.

The second scenario deals with applications where more information is known (level 3 above). Therefore, they provide hand-curated collections of known matches.

In the first scenario we start running the Data Tamer system, asking a human for help as appropriate. In the second scenario, we make use of known duplicates as initial training data.

### Data Source Update

Finally, some data sources may be dynamic, and be continually updated. In this case, Data Tamer can make a new snapshot of a previous data source-k.

## Data Tamer Components

A block diagram of Data Tamer is shown in Figure 1. Indicated in the diagram are the management console and components for schema integration, entity consolidation, DE support and human transformation. These four subsystems are described in this section. Most of the features described in this section are currently operational.

![此处输入图片的描述][1]

### Schema Integration

The basic inner loop in schema integration is to ingest an attribute, Ai from a data source and compare it to a collection of other attributes in a pairwise fashion.

Our approach is to use a collection of algorithms, which we term experts, each returning a score between 0 and 1. Afterwards, the scores are consolidated with a set of weights to produce a composite value.

The attribute mappings to be considered by Data Tamer depend on what information is available for the curation problem at hand,

### Entity Consolidation

Entity consolidation is effectively modeled as duplicate elimination. The goal is to find entities that are similar enough to be considered duplicates. This module receives a collection of records, R1, ..., Rn, from one or more local data sources that arrive incrementally.

 - Bootstrapping the Training Process   
 Initially, the system starts with zero knowledge about the deduplication rules. We learn deduplication rules from a training set of known duplicates and non-duplicates.
 - Categorization of Records   
 Records are classified into multiple categories such that each category represents a set of homogenous entities that have similar non-null attributes and similar attribute values.
 - Learning Deduplication Rules   
 The deduplication rules are divided into two types: (1) cut-off thresholds on attribute similarities, which help pruning a large number of tuple pairs; and (2) probability distributions of attribute similarities for duplicate and non-duplicate tuple pairs. We learn such rules from the collected training data TP and TN.
 - Similarity Join   
 We obtain all candidate tuple pairs, where each pair belong to the same category and at least one attribute has a similarity above its learned threshold. Then, we compute the attribute similarities of the candidate pairs and we use these similarities to identify duplicate records according to the classifier learned.
 - Record Clustering and Consolidation   
 Once we obtain a list of tuple pairs that are believed to be duplicates, we need to obtain a clustering of tuples such that each cluster represents a distinct real-world entity.

## Human Interface

In both the attribute identification phase and the entity consolidation phase, a human DE may be asked by a DTA to provide curation input. In the case of attribute identification, the task is to decide if two attributes are the same thing or not. In the case of entity resolution, the task is to decide if two entities are duplicates or not.

 - Manual Mode   
 If the tasks requiring human intervention are few or if there are only a few DEs, then the DTA can manually assign human tasks to DEs. 
 - Crowd Sourcing Mode   
 Large-scale data curation may require enlisting additional DEs with lesser expertise to help with the workload. Non-guru DEs can be asked to complete "easier" tasks, or they could be crowdsourced to produce results with higher confidence of correctness than could be assumed of any of them individually.

## Visualization Component

At any time, the DTA or a DE can call our visualization system and pass a local data source to that system. It displays the data source (or a sample of the source) as a table on the screen. The human can inspect the data source for insight.

***相关连接***

 - https://cs.uwaterloo.ca/~ilyas/papers/StonebrakerCIDR2013.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2000-01-01-data_curation/2000-01-01-data_curation_1.png
  