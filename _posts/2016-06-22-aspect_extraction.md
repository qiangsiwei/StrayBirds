---
layout: post
title: Aspect Extraction
category: Notes
comments: true
---

# Aspect Extraction

------

### Aspect Extraction

The key characteristic is that an opinion always has a target. The target is often the aspect or topic to be extracted from a sentence. Thus, it is important to recognize each opinion expression and its target from a sentence. However, we should also note that some opinion expressions can play two roles, i.e., indicating a positive or negative sentiment and implying an (implicit) aspect (target).

There are four main approaches:

 - Extraction based on frequent nouns and noun phrases
 - Extraction by exploiting opinion and target relations
 - Extraction using supervised learning
 - Extraction using topic modeling

## Finding Frequent Nouns and Noun Phrases

This method finds explicit aspect expressions that are nouns and noun phrases from a large number of reviews in a given domain. Hu and Liu (2004) used a data mining algorithm. Nouns and noun phrases (or groups) were identified by a part-of-speech (POS) tagger.

The reason that this approach works is that when people comment on different aspects of an entity, the vocabulary that they use usually converges.

The precision of this algorithm was improved in (Popescu and Etzioni, 2005). Their algorithm tried to remove those noun phrases that may not be aspects of entities. It evaluated each discovered noun phrase by computing a pointwise mutual information (PMI) score between the phrase and some meronymy discriminators associated with the entity class, e.g., a camera class.

![此处输入图片的描述][1]

where a is a candidate aspect identified using the frequency approach and d is a discriminator. Web search was used to find the number of hits of individual terms and also their co-occurrences. The idea of this approach is clear. If the PMI value of a candidate aspect is too low, it may not be a component of the product because a and d do not co-occur frequently.

## Using Opinion and Target Relations

Since opinions have targets, they are obviously related. Their relationships can be exploited to extract aspects which are opinion targets because sentiment words are often known. This method was used in (Hu and Liu, 2004) for extracting infrequent aspects. The idea is as follows: The same sentiment word can be used to describe or modify different aspects. This idea turns out to be quite useful in practice even when it is applied alone.

In (Zhuang, Jing and Zhu, 2006), a dependency parser was used to identify such dependency relations for aspect extraction. Somasundaran and Wiebe (2009) employed a similar approach, and so did Kobayashi et al. (Kobayashi et al., 2006). The dependency idea was further generalized into the double-propagation method for simultaneously extracting both sentiment words and aspects in (Qiu et al., 2011).

## Using Supervised Learning

Many algorithms based on supervised learning have been proposed in the past for information extraction (Hobbs and Riloff, 2010; Mooney and Bunescu, 2005; Sarawagi, 2008). 

The most dominant methods are based on sequential learning (or sequential labeling). Since these are supervised techniques, they need manually labeled data for training. That is, one needs to manually annotate aspects and non-aspects in a corpus. The current state-of-the-art sequential learning methods are Hidden Markov Models (HMM) (Rabiner, 1989) and Conditional Random Fields (CRF) (Lafferty, McCallum and Pereira, 2001).

A set of domain independent features were also used, e.g. tokens, POS tags, syntactic dependency, word distance, and opinion sentences. Li et al (2010) integrated two CRF variations, i.e., Skip-CRF and Tree-CRF, to extract aspects and also opinions.

## Using Topic Models

In recent years, statistical topic models have emerged as a principled method for discovering topics from a large collection of text documents. Topic modeling is an unsupervised learning method that assumes each document consists of a mixture of topics and each topic is a probability distribution over words. A topic model is basically a document generative model which specifies a probabilistic procedure by which documents can be generated. The output of topic modeling is a set of word clusters. Each cluster forms a topic and is a probability distribution over words in the document collection.

There were two main basic models, pLSA (Probabilistic Latent Semantic Analysis) (Hofmann, 1999) and LDA (Latent Dirichlet allocation) (Blei, Ng and Jordan, 2003; Griffiths and Steyvers, 2003; Steyvers and Griffiths, 2007).

Intuitively topics from topic models are aspects in the sentiment analysis context. Topic modeling can thus be applied to extract aspects. However, there is also a difference. That is, topics can cover both aspect words and sentiment words.

Although topic modeling is a principled approach based on probabilistic inferencing and can be extended to model many types of information, it does have some weaknesses which limit its practical use in real-life sentiment analysis applications. 

One main issue is that it needs a large volume of data and a significant amount of tuning in order to achieve reasonable results. To make matters worse, most topic modeling methods use Gibbs sampling, which produces slightly different results in different runs due to MCMC (Markov chain Monte Carlo) sampling, which makes parameter tuning time consuming. 

While it is not hard for topic modeling to find those very general and frequent topics or aspects from a large document collection, it is not easy to find those locally frequent but globally not so frequent aspects. Such locally frequent aspects are often the most useful ones for applications because they are likely to be most relevant to the specific entities that the user is interested in. Those very general and frequent aspects can also be easily found by the methods discussed earlier. 

One promising research direction is to incorporate more existing natural language and domain knowledge in the models. There are already some initial works in this direction (Andrzejewski and Zhu, 2009; Andrzejewski, Zhu and Craven, 2009; Mukherjee and Liu, 2012; Zhai et al., 2011).

## Mapping Implicit Aspects

Although explicit aspect extraction has been studied extensively, limited research has been done on mapping implicit aspects to their explicit aspects. 

In (Su et al., 2008), a clustering method was proposed to map implicit aspect expressions, which were assumed to be sentiment words, to their corresponding explicit aspects. The method exploits the mutual reinforcement relationship between an explicit aspect and a sentiment word forming a co-occurring pair in a sentence.

In (Hai, Chang and Kim, 2011), a two-phase co-occurrence association rule mining approach was proposed to match implicit aspects (which are also assumed to be sentiment words) with explicit aspects. 

***相关连接***

 - https://www.cs.uic.edu/~liub/FBS/SentimentAnalysis-and-OpinionMining.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-22-aspect_extraction/2016-06-22-aspect_extraction_1.png
  