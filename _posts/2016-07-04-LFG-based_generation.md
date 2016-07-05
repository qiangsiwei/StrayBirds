---
layout: post
title: LFG-Based Generation
category: Grammar Formalisms
comments: true
---

# LFG-Based Generation

------

### LFG-Based Generation

Sentence generation, or surface realisation can be described as the problem of producing syntactically, morphologically, and orthographically correct sentences from a given abstract semantic / logical representation according to some linguistic theory, e.g. Lexical Functional Grammar (LFG), Head-Driven Phrase Structure Grammar (HPSG), Combinatory Categorial Grammar (CCG), Tree Adjoining Grammar (TAG) etc. 

Grammars, such as these, are declarative formulations of the correspondences between semantic and syntactic representations. Grammar rules have been carefully handcrafted, such as those used in LinGo (Carroll et al., 1999), OpenCCG (White, 2004) and XLE (Crouch et al., 2007).

As handcrafting grammar rules is time-consuming, language-dependent and domain-specific, recent years have witnessed research on extracting wide-coverage grammars automatically from annotated corpora, for both parsing and generation.

In addition to applying statistical techniques to automatically acquire generation grammars, over the last decade, there has been a lot of interest in a generate-and-select paradigm for surface realisation. 

The paradigm is characterised by a separation between generation and selection, in which symbolic or rule-based methods are used to generate a space of possible paraphrases, and statistical methods are used to select one or more outputs from the space.

It is interesting to note that, while the study of how the granularity of context-free grammars (CFG) affects the performance of a parser (e.g. in the form of grammar transforms (Johnson, 1998) and lexicalisation (Collins, 1997)) has attracted substantial attention, to our knowledge, there has been a lot less research on this subject for surface realisation, a process that is generally regarded as the reverse process of parsing.

## Lexical Functional Grammar

Lexical Functional Grammar (Kaplan and Bresnan, 1982) is a constraint-based grammar formalism which postulates (minimally) two levels of representation: c(onstituent)-structure and f(unctional)-structure. 

C-structure takes the form of phrase structure trees and captures surface grammatical configurations. F-structure encodes more abstract grammatical functions (GFs) such as SUBJ(ect), OBJ(ect), ADJUNCT and TOPIC etc., in the form of hierarchical attribute-value matrices. 

C-structures and f-structures are related by a piecewise correspondence function φ that goes from the nodes of a c-structure tree into units of f-structure spaces (Kaplan, 1995).

C- and f-structures with φ links for the sentence "江泽民会见泰国总理"

![此处输入图片的描述][1]

(1) shows a miniature set of annotated CFG rules (lexical entries omitted) which generates the c- and f-structure.

![此处输入图片的描述][2]

In the functional annotations, (↓) refers to the f-structure associated with the local c-structure node ni, i.e. φ(ni), and (↑) refers to the f-structure associated with the mother (M) node of ni, i.e. φ(M(ni)).

## Generation from f-Structures

The LFG-based statistical generation model defines the conditional probability P(T|F), for each candidate functionally annotated c-structure tree T (which fully specifies a surface realisation) given an f-structure F.

P(T|F) is then decomposed as the product of the probabilities of all the functionally annotated CFG rewriting rules X → Y (conditioned on the left hand side (LHS) X and local features of the corresponding f-structure φ(X)) contributing to the tree T (Eq. 2).

![此处输入图片的描述][3]

## Disambiguation Models

The basic generation model presented in (Cahill and van Genabith, 2006) used simple probabilistic context-free grammars. However, the independence assumptions implicit in PCFG models may not be appropriate to best capture natural language phenomena.

### A History-Based Model

The history-based (HB) approach which incorporates more context information has worked well in parsing (Collins, 1997; Charniak, 2000).

The history-based model increases the context by simply including the parent grammatical function GF of the f-structure in addition to the local φ-linked feature set in the conditioning context (Eq. 3).

![此处输入图片的描述][4]

Though Chinese does not distinguish cases, the f-structure parent GF are expected to help predict grammar rule expansions more accurately in the tree derivation than the simple PCFG model.

### A Lexicalised Model

Compared to the HB model which includes the parent grammatical function in the conditioning context, lexicalised grammar rules contain more fine-grained categorial information.

The expectation is that a lexicalised PCFG model also works better than a simple PCFG model in Chinese generation, considering e.g. prepositional phrase (PP) modification in Chinese.

![此处输入图片的描述][5]

In order to model phenomena such as these, we head-lexicalise our grammar by associating each non-terminal node with the head word2 in the c-structure tree along the head-projection line. 

To handle the problem of sparse data while estimating rule probabilities, a back-off to baseline model is employed. As, from a linguistic perspective, it is the modifier rather than the head word which plays the main role in determining word order, a back-off to partial lexicalisation on the modifier only is also used for binary rules.

![此处输入图片的描述][6]

## Chart Generation

The PCFG-based generation algorithms are implemented in terms of a chart generator (Kay, 1996). In the generation algorithm, each (sub-)f-structure indexes a (sub-)chart. Each local chart generates the most probable trees for the local f-structure in a bottom-up manner:

 - generating lexical edges from the the local GF PRED and some atomic features representing function words, mood or aspect etc.
 - applying unary rules and binary rules to generate new edges until no any new edges can be generated in the current local chart.
 - propagating compatible edges to the upper-level chart.

***相关连接***

 - http://www.aclweb.org/anthology/W08-1112

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-07-04-LFG-based_generation/2016-07-04-LFG-based_generation_6.png
