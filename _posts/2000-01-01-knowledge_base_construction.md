---
layout: post
title: Data Labeling in Knowledge Base Construction
category: Systems
comments: true
---

# Mindtagger: A Demonstration of Data Labeling in Knowledge Base Construction

------

As a result of deploying DeepDive framework across several domains, new challenges are found in debugging and improving such end-to-end systems to construct high-quality knowledge bases.

To help users, it is needed to develop principles for analyzing the system’s error as well as provide tooling for inspecting and labeling various data products of the system.

To enable more productive and systematic data labeling, Mindtagger was created, a versatile tool that can be configured to support a wide range of tasks.

Several groups in different domains are using DeepDive and Mindtagger to construct high-quality knowledge bases: paleo-biology, genomics, pharma domains, intelligence, lawenforcement, and material science.

End-to-end knowledge base construction (KBC) systems using statistical inference are enabling more people to automatically extract high-quality domain-specific information from unstructured data.

A wide range of data processing components are put together in the system: crawlers pull in large amounts of unstructured text and image data from various sources; ETL (extract, transform, and load) components clean the raw data and add natural language processing markups to the text; an array of extractors extract candidate mentions of entities and relationships along with their features; an inference engine constructs and trains a probabilistic model from the data to predict probabilities of the extracted information; finally, a search engine surfaces such information to law enforcement personnel.

## Challenges

### Principled Error Analysis

In many cases, colleagues were tempted to examine only a small sample of errors and to use their intuition and luck to fix whichever attractive ones they encountered.

Mindtagger created guidelines for error analysis that help users identify possible improvements and assess their potential impact in a principled way.

### Productive Data Labeling

Lack of tooling for inspecting and labeling data products of the system under development was slowing down every iteration of the development cycle. In every step of error analysis guidelines, data labeling plays a key role.

To enable more productive data labeling and to study unanticipated types of labeling tasks involved in the actual error analysis, Mindtagger was created, an interactive graphical tool for labeling data. 

Innovative systems such as Data Tamer and Trifacta are making human involvement more productive in data integration, cleaning, and transformation, but Mindtagger is richer in the sense that end-to-end KBC systems typically handle those problems as part of the statistical inference.

### Variable Data Products and Labeling Tasks

The data to be inspected as well as the detail of necessary labeling tasks constantly vary as development progresses.

Mindtagger support a wide range of tasks and data types by mixing and matching predefined configuration fragments so that the task configurations could be easily reused and extended.

## System Overview

Mindtagger is an interactive data labeling tool we created to help DeepDive users perform error analysis in more systematic and productive ways. 

Mindtagger takes as input a list of data items and a template that defines the presentation of, and possible interactions with the items and provides a graphical user interface for annotating labels while browsing them.

Figure 1 shows where Mindtagger and the error analysis steps it supports fit into the overall iterative development cycle of an end-to-end knowledge base construction system.

![此处输入图片的描述][1]

Mindtagger can be easily configured to support a wide range of data labeling tasks by customizing one of its predefined modes: precision, full markup recall, quick check recall, and inspection mode, some of which are shown in Figure 2.

![此处输入图片的描述][2]

## Demonstration Details

A small part of a hypothetical knowledge base construction system is used to demonstrate how Mindtagger supports the data labeling tasks necessary for a full error analysis cycle. It boils down to following these three steps after every iteration:

 - Evaluate the precision and/or recall of the extractions by identifying errors from a sample of the data product.
 - Inspect and cluster the errors with their immediate details, such as extracted features.
 - Explore extra data products relevant to the most common errors to develop concrete ideas for improvement.

### Phone Number Extraction Example

It is assumed that the information extraction is done using a simple binary classifier, and the following four components are in the system:

 - Candidate Extraction. A basic candidate extractor scans the full text of the web pages with a set of simple regular expressions and emits matching text-spans that are likely to be phone numbers.
 - Feature Extraction. Then a feature extractor collects words and phrases appearing near each candidate as its features for classification.
 - Supervision. Instead of manually curating a training set, a program with a set of rules collects candidates as positive training examples that appear in a sentence that contains words highly likely to appear next to phone numbers, such as "call", "phone" or "contact".
 - Inference. Finally, the binary classifier uses logistic regression to learn and predict whether the text-span is a phone number or not.

### Evaluating Precision

The estimated precision is simply the ratio of the number of true positives to the total number of samples examined.

### Evaluating Recall

Estimating recall is slightly more challenging than precision, as it’s about the unseen part of the data. The foremost objective of evaluating recall is to discover false negatives from the samples drawn from the entire corpus, not from the output of the extractor or classifier. The estimated recall is then the reciprocal of 1 + the relative size of false negatives to true positives in the sample.

 - Full markup mode is designed for carefully identifying all mentions appearing in sampled documents.
 - Quick check mode is for tagging whether a sentence or paragraph simply contains a mention or not.

### Inspecting Errors

It is assumed that the attendee wants to improve the extraction quality after evaluating precision and recall, although error analysis in general can stop here when the measures are satisfactory. She now has to understand the cause for each error found in the previous steps in order to cluster them and focus on the most common type.

The attendee browses the errors partially labeled with their causes in Mindtagger’s inspection mode using the configuration below. She can add her own labels to the text mistaken as phone numbers, such as "part of URL" or "part of product code", which hints at how the regular expressions should be tuned or what new features should be extracted.

### Calibrating Supervision

To correct the inspected errors, it is often necessary to explore more data that share features with the errors to calibrate the supervision rules. False positives can be explained by features having spuriously high weights learned from a biased training set with little negative examples but too many positive ones. Taking a broader look at more candidates sharing such features helps in understanding which rules were overly supervising the candidates as positive training examples, or more importantly, what new rules could be created to turn some of the unsupervised candidates into negative training examples. Similarly, false negatives due to low-weighted features can be handled by calibrating the supervision rules to introduce more positive examples after examining other candidates sharing the features.

Using Mindtagger’s inspection mode, the attendee can reliably come up with new rules for negative training examples based on concrete observations.

***相关连接***

 - http://cs.stanford.edu/people/chrismre/papers/p2148-shin-vldb-demo.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2000-01-01-knowledge_base_construction/2000-01-01-knowledge_base_construction_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2000-01-01-knowledge_base_construction/2000-01-01-knowledge_base_construction_2.png
  