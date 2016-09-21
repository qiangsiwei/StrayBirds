---
layout: post
title: Big Data Information System
category: Systems
comments: true
---

# An Information Provider’s Wish List for a Next Generation Big Data End-to-End Information System

------

As the leading source of intelligent information, Thomson Reuters delivers must-have insight to the world’s financial and risk, legal, tax and accounting, intellectual property, science and media professionals, supported by the world's most trusted news organization.

<http://cidrdb.org/cidr2015/Papers/CIDR15_Paper32.pdf>

## Motivation

Recently, the company set an enterprise strategy with a focus on new enterprise opportunities in addition to those within each business unit. To align the data management strategy to the business strategy, Thomson Reuters launched an initiative, called the Big Data Initiative for the purpose of this paper, to store all Thomson Reuters data, and adopt open standards for linking the contents of this data.

A consistent, robust, accurately and reliably updated entity aggregation protocol is an enterprise-wide core requirement that enables Thomson Reuters products to provide accurate and intuitive content navigation. Thomson Reuters currently provides both deep and broad content across a large spectrum of information at considerable cost. To find and use this information, there must be a working mechanism that can easily search, navigate and successfully find and retrieve any and all desired information or content. This functionality far exceeds typical full text searching and enables customers to understand and interpret the complex relationships of organizations in today’s financial and professional landscape.

It is critical to deployment efficiency that these mechanisms be a global shared service across the enterprise, available to all systems, including both content and products.

## A Big Data Initiative

Thomson Reuters’ Big Data initiative is an enterprise level infrastructure that aims to gather, aggregate, connect and disseminate Thomson Reuters large and varied content in addition to allowing useful linking with customer, external partner and open web content. The Big Data initiative enables automated content collection, leverages commodity hardware storage, and increasingly cheaper processing. By enabling distributed, scalable computation alongside the data, this infrastructure provides tools to enable new insights into the data through machine learning, predictive analytics, visualization and natural language querying. This initiative is a response to changing customer needs and expectations with respect to integrating multiple content sources, including customers’ own proprietary, third party, open and social media data. The ultimate goal of the Big Data initiative is facility building analytics and visualization on top of diverse data sources.

![此处输入图片的描述][1]

With this Big Data initiative, Thomson Reuters is extending the master data management and service oriented architectures approach that have been in place in order to meet four objectives:

 - A desire across Thomson Reuters to use new Big Data technologies to solve existing and new problems.
 - Overcoming difficulty in accessing data from across the entire organization. Data from around Thomson Reuters can still be siloed in stores dedicated to the products that deliver it.
 - The fact that applying Big Data technologies requires a lot of perspiration before the innovation can happen. Thomson Reuters wants to provide a sandbox that business units can jump into and use.
 - The challenge of emerging open data standards. The rise of open data with emerging de facto standards such as opencorporates.org, schema.org, in addition to commercial standards such as OMG’s Financial Industry Business Ontology (FIBO) poses an opportunity: What data does Thomson Reuters have that can be opened up to drive customer innovation? How do we deliver to our customers the ability to leverage open data and link to many types of external data and internal data accurately?

Within this Big Data initiative, Thomson Reuters is building four key capabilities:

 - A Data Lake of mostly diverse content available in Hadoop, with automatic ingest from Novus, Content Marketplace and other potentially external sources as necessary, including open data. Business units can freely access the content and schedule their own jobs to run on the content as it becomes available.
 - A Linked Data store, the Knowledge Graph, which represents our knowledge of the interconnectedness of entities within the content.
 - Data Marts that take slices of data from the Lake and Graph to solve particular business problems. Neither the Lake nor the Knowledge Graph can be optimized for every possible use case. The Data Marts solve for this.
 - Metadata as a Service (MDaaS), a web application and associated APIs to support open data.

The Lake is (currently) a Hadoop-based consolidated data store of content with compute alongside store of Thomson Reuters content that enables Map/Reduce and SPARK processing. The Lake automatically ingests Novus, Content Marketplace and other content of interest via a landing strip using a scheduler for periodic or on-availability jobs. An ingestion system based on the Apache Kafka messaging framework is used to load data into the Lake.

Above the Lake is a job scheduling capability, allowing developers to configure jobs on a per content type basis: for example, the scheduler may be programmed to run a certain set of jobs every time a new patent is available. It is envisioned that there will be a library of pre-existing jobs for entity extraction, mapping strings of text to entities in the entity master lists. Every loaded data item is assigned a URI for downstream representation in the Knowledge Graph. Currently, no customer data or PII is being ingested into the Lake. Big Data infrastructures raise many familiar and novel privacy concerns that Thomson Reuters is committed to handling responsibly.

A typical workflow for a user of the Lake might go something like this: Using a search interface to Novus, or other repository, someone interested in processing data using the Big Data infrastructure could use GUIDs associated with a particular entity of interest to identify content collections that contain the relevant GUID. Once the collection is identified, sample XML can be examined. XML elements of interest can be looked up in entity masters to understand what they mean. Then, the data can be ingested into the Lake. Once there, a Hive query can be used to extract attributes of interest by means of XPath specification of the elements of the XML. The extracted and columnized data can then be used as the basis for visualizations, joins, downloaded or used for further analysis.

## Open Linked Data & Knowledge Graphs

The Knowledge Graph is a highly scalable link data store, using REST standards for input and output, built on Cassandra and Elastic Search over a cluster of servers. The Knowledge Graph represents relationships between known entities extracted from the content of the Lake as well as those represented within Thomson Reuters’ authority databases for people and organizations initially. The Knowledge Graph enables content retrieval by URI, Search, and pub/sub mechanisms. The ambition here is to capture the knowledge encoded about entities in our content organized around core entity types like company and represented in the form of a graph, rather than as tables or XML.

The problem of mapping text sequences with entities in a master entity list in order to populate such a Knowledge Graph is extremely common with Thomson Reuters data.

## Envisioned Future State

While it is valuable to be able to perform computations easily at scale in the Big Data architecture we have sketched and to extract relationships from that data into a Knowledge Graph, that can be searched, queried, and analyzed alongside other graph-based representations of data, joined on common identifiers, some additional capabilities are desired.

### INFORMATION INTEGRATION

A major issue with combining datasets for analytical purposes is identifying the same entity across content. For internal data that has strictly followed our entity management protocols, this is not an issue; each instance of an entity type has a unique identifier, and all the XML elements that use that identifier are well-documented. However, when not even all our internal and diverse data strictly conforms to our protocols, external data cannot be expected to conform to it. 

### ACCESSIBILITY

Currently, data is being ingested into the Lake, but it is difficult to know what data has been ingested without consulting ingestion schedules.

### CURATION

Currently, data is ingested into the Lake and then abstracted into the Knowledge Graph only after a considerable amount of editorial curation occurs upstream of the Lake. It would be preferable if the data entered the Lake in its raw state, and Big Data techniques were used to automate, at least in part, the editorial curation.

### PROVENANCE TRACKING

Ultimately, we would like to be able to track the provenance of information all the way back to the first time that we encounter it. This includes all content types including structured databases, documents, video files etc.

### RIGHTS AND PERMISSIONS

An important need is the ability and to express and enforce what can be done to content. There may be other future considerations that required a flexible and scalable technology solution to expression and enforcement of rights and permissions such as adding novel types of external contents.

### KNOWLEDGE GRAPH

The Lake contains many, many relationships between entities that can be identified and categorized by Thomson Reuters’ named entity recognition engines. It is not clear whether all of these relationships should be represented in the Knowledge Graph or only some. Representing temporal information within the Graph is also a challenge. Knowledge Graphs can grow very large, and we have been having trouble identifying technologies that can enable representing, querying and performing analytics on graphs of the size that we can extract from just one subset of content in the Lake.

### DATA VISUALIZATION

Current methods for visualizing document sets and the connections between them are lacking. Visualizations require a great deal of effort to set up, such that exploring datasets for those outside of the group that normally curates it is difficult.

***相关连接***

 - http://cidrdb.org/cidr2015/Papers/CIDR15_Paper32.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-08-big_data_information_system/2016-08-08-big_data_information_system_1.png
  