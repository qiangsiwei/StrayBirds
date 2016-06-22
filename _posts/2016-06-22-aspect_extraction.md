---
layout: post
title: Aspect Extraction
category: Notes
comments: true
---

# Aspect Extraction

------

### Aspect Extraction

Aspect-based Sentiment Analysis问题的另一子问题是Aspect Extraction，然而并不是所有的情感都有明确指向，一方面情感的指向可能是隐含的（谓词隐含了主或宾），另一方面可能是对事物的笼统层面（the GENERAL aspect）。

The key characteristic is that an opinion always has a target. The target is often the aspect or topic to be extracted from a sentence. Thus, it is important to recognize each opinion expression and its target from a sentence. However, we should also note that some opinion expressions can play two roles, i.e., indicating a positive or negative sentiment and implying an (implicit) aspect (target).

Aspect Extraction主要包含下面几种方法。

There are four main approaches:

 - Extraction based on frequent nouns and noun phrases
 - Extraction by exploiting opinion and target relations
 - Extraction using supervised learning
 - Extraction using topic modeling

## Finding Frequent Nouns and Noun Phrases

情感的指向一般是事物的某个方面，因此一般是名词或名词短语（Nouns or Noun Phrases），因此Aspect Extraction最简单的方法是基于词性标注（part-of-speech (POS) tagger）抽取出高频名词或名词短语。

This method finds explicit aspect expressions that are nouns and noun phrases from a large number of reviews in a given domain. Hu and Liu (2004) used a data mining algorithm. Nouns and noun phrases (or groups) were identified by a part-of-speech (POS) tagger.

The reason that this approach works is that when people comment on different aspects of an entity, the vocabulary that they use usually converges.

该方法可基于pointwise mutual information (PMI)进行改进，提高抽取的准确率。

The precision of this algorithm was improved in (Popescu and Etzioni, 2005). Their algorithm tried to remove those noun phrases that may not be aspects of entities. It evaluated each discovered noun phrase by computing a pointwise mutual information (PMI) score between the phrase and some meronymy discriminators associated with the entity class, e.g., a camera class.

pointwise mutual information (PMI)的计算如下。

![此处输入图片的描述][1]

where a is a candidate aspect identified using the frequency approach and d is a discriminator. Web search was used to find the number of hits of individual terms and also their co-occurrences. The idea of this approach is clear. If the PMI value of a candidate aspect is too low, it may not be a component of the product because a and d do not co-occur frequently.

## Using Opinion and Target Relations

Aspect Extraction的另一种方法是基于观点（Opinion）和目标（Target）的关系（Relations）。

举个简单的例子，观点"好吃"的目标一般是某种事物，因此关系也可以理解成搭配。

Since opinions have targets, they are obviously related. Their relationships can be exploited to extract aspects which are opinion targets because sentiment words are often known. This method was used in (Hu and Liu, 2004) for extracting infrequent aspects. The idea is as follows: The same sentiment word can be used to describe or modify different aspects. This idea turns out to be quite useful in practice even when it is applied alone.

基于常识，观点和目标的搭配通常比较有限和固定，因此采用该种方法一般能够达到比较好的效果。

由于长期依赖性问题的存在，可基于依存语法树（dependency parser）更准确的抽取出关系，因此能够提高抽取的准确率。

In (Zhuang, Jing and Zhu, 2006), a dependency parser was used to identify such dependency relations for aspect extraction. Somasundaran and Wiebe (2009) employed a similar approach, and so did Kobayashi et al. (Kobayashi et al., 2006). The dependency idea was further generalized into the double-propagation method for simultaneously extracting both sentiment words and aspects in (Qiu et al., 2011).

## Using Supervised Learning

Aspect Extraction的第三种方法是基于有监督的学习（Supervised Learning）。

Many algorithms based on supervised learning have been proposed in the past for information extraction (Hobbs and Riloff, 2010; Mooney and Bunescu, 2005; Sarawagi, 2008). 

有监督的学习算法最常用的是各种序列标注算法（sequential learning or sequential labeling），包括隐含马尔科夫（Hidden Markov Models (HMM)）和条件随机场（Conditional Random Fields (CRF)）等。

The most dominant methods are based on sequential learning (or sequential labeling). Since these are supervised techniques, they need manually labeled data for training. That is, one needs to manually annotate aspects and non-aspects in a corpus. The current state-of-the-art sequential learning methods are Hidden Markov Models (HMM) (Rabiner, 1989) and Conditional Random Fields (CRF) (Lafferty, McCallum and Pereira, 2001).

有监督的学习都需要确定所采用的特征，特征的优劣直接关系到效果的好坏，常用的特征包括：词（tokens）、词性（POS tags），句法依存关系（syntactic dependency），词距（word distance）等。HMM和CRF也包括各种变型，提高抽取的准确率。

A set of domain independent features were also used, e.g. tokens, POS tags, syntactic dependency, word distance, and opinion sentences. Li et al (2010) integrated two CRF variations, i.e., Skip-CRF and Tree-CRF, to extract aspects and also opinions.

## Using Topic Models

Aspect Extraction的第四种方法是基于主题模型。

In recent years, statistical topic models have emerged as a principled method for discovering topics from a large collection of text documents. Topic modeling is an unsupervised learning method that assumes each document consists of a mixture of topics and each topic is a probability distribution over words. A topic model is basically a document generative model which specifies a probabilistic procedure by which documents can be generated. The output of topic modeling is a set of word clusters. Each cluster forms a topic and is a probability distribution over words in the document collection.

主题模型主要包含两种主流的模型，即pLSA和LDA。

There were two main basic models, pLSA (Probabilistic Latent Semantic Analysis) (Hofmann, 1999) and LDA (Latent Dirichlet allocation) (Blei, Ng and Jordan, 2003; Griffiths and Steyvers, 2003; Steyvers and Griffiths, 2007).

主题模型的解释如下。

Intuitively topics from topic models are aspects in the sentiment analysis context. Topic modeling can thus be applied to extract aspects. However, there is also a difference. That is, topics can cover both aspect words and sentiment words.

主题模型基于概率进行推理（inferencing），因此具有良好的可扩展性和解释性，然而也具有明显的劣势。

Although topic modeling is a principled approach based on probabilistic inferencing and can be extended to model many types of information, it does have some weaknesses which limit its practical use in real-life sentiment analysis applications. 

第一个问题是所需的数据量大，计算时间长，同时是基于Gibbs sampling的近似方法的结果可能具有一定的偏差。

One main issue is that it needs a large volume of data and a significant amount of tuning in order to achieve reasonable results. To make matters worse, most topic modeling methods use Gibbs sampling, which produces slightly different results in different runs due to MCMC (Markov chain Monte Carlo) sampling, which makes parameter tuning time consuming. 

第二个问题是主题模型基于词袋模型，因此不能很好的捕捉低频词，并很好的利用句法特征。

While it is not hard for topic modeling to find those very general and frequent topics or aspects from a large document collection, it is not easy to find those locally frequent but globally not so frequent aspects. Such locally frequent aspects are often the most useful ones for applications because they are likely to be most relevant to the specific entities that the user is interested in. Those very general and frequent aspects can also be easily found by the methods discussed earlier. 

然后不少研究仍然在对主题模型进行改进，以期待提高抽取的准确率。

One promising research direction is to incorporate more existing natural language and domain knowledge in the models. There are already some initial works in this direction (Andrzejewski and Zhu, 2009; Andrzejewski, Zhu and Craven, 2009; Mukherjee and Liu, 2012; Zhai et al., 2011).

## Mapping Implicit Aspects

然而并不是所有的情感都有明确指向，一方面情感的指向可能是隐含的（谓词隐含了主或宾），另一方面可能是对事物的笼统层面（the GENERAL aspect）。

因此，在Aspects隐含或缺失的时候需要对其进行补足。

Although explicit aspect extraction has been studied extensively, limited research has been done on mapping implicit aspects to their explicit aspects. 

常用的方法可基于聚类算法、共现等。

In (Su et al., 2008), a clustering method was proposed to map implicit aspect expressions, which were assumed to be sentiment words, to their corresponding explicit aspects. The method exploits the mutual reinforcement relationship between an explicit aspect and a sentiment word forming a co-occurring pair in a sentence.

In (Hai, Chang and Kim, 2011), a two-phase co-occurrence association rule mining approach was proposed to match implicit aspects (which are also assumed to be sentiment words) with explicit aspects. 

***相关连接***

 - https://www.cs.uic.edu/~liub/FBS/SentimentAnalysis-and-OpinionMining.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-22-aspect_extraction/2016-06-22-aspect_extraction_1.png
  