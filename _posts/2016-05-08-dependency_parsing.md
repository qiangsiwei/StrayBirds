---
layout: post
title: 生成文法
category: Notes
comments: true
---

# 生成文法

------

### 生成文法

在理论语言学中，生成文法（generative grammar）是一种尝试接近语法学（Syntax）的方式 。生成文法尝试给出一套规则，其能正确的预测，在一个语言中，什么样的词汇组合能成为正确的句子；而在讨论生成文法的同时，这些规则通常也能预测句子中的构词法。  

生成文法源于50年代末语言学家乔姆斯基的研究工作（他的理论在较早的版本里叫做转换文法（transformational grammar; TGG）。   
这个词现在作为集合名词，指此理论以及其后继）。   
而后来也有各种版本的生成语法理论与之争鸣。   
乔姆斯基目前的理论称作“最简方案”（Minimalist Program; MP）。   
其他著名的理论包括主辞驱动句构造文法（Head-driven phrase structure grammar; HPSG），   
语汇机能文法（Lexical functional grammar; LFG），   
范畴文法（Categorial grammar; CG），   
关系文法（Relational grammar; RG），   
以及树-邻接文法（Tree-adjoining grammar; TAG）。

乔姆斯基认为，生成文法中的性质来自一种“天生的”、普适的语法。提倡生成文法的学者认为，大多数的语法并不是由于交际功能而产生的，也不是简单地从环境中学得的（见刺激贫乏论）。在这方面，生成文法中的观点与认知文法，功能主义或行为主义的理论都有所区别。
 
**Formalisms define different representations 各种流派**

 - Tree-adjoining Grammar (TAG):   
    Fragments of phrase-structure trees   
 - Lexical-functional Grammar (LFG):   
    Annotated phrase-structure trees (c-structure)   
    linked to feature structures (f-structure)   
 - Combinatory Categorial Grammar (CCG):   
    Syntactic categories paired with meaning representations   
 - Head-Driven Phrase Structure Grammar(HPSG):   
    Complex feature structures (Attribute-value matrices)   

**Lexicalization 词汇化**  

No lexicalization: (CFG) 

 - The lexicon contains little syntactic information (e.g. just POS-tags) –  Recursion is entirely defined by language-specific grammar rules   

Weak lexicalization: (LFG)   

 - The lexicon (and lexical rules) specify some language-specific information (e.g. subcategorization, semantics, control, binding theory, passivization)   
 - Recursion is defined by language-specific grammar rules (but lexical information may constrain which rules can be used in which context)   

Strong lexicalization: (TAG, CCG, HPSG)   

 - The lexicon (and lexical rules) specifies all language-specific information (e.g. word order, subcategorization, semantics, control, binding theory)   
 - The lexicon pairs words with complex elementary objects These objects may have an extended domain of locality (i.e. capture structure beyond a single CFG rule)   
 - Recursion is defined by completely universal operations   

详细内容参见：
<http://nlp.cs.illinois.edu/HockenmaierGroup/ACL2010Tutorial/ACL2010TutorialGrammars.pdf>


### 语法分析器

在计算机科学和语言学中，语法分析（Syntactic analysis，Parsing）是根据某种给定的形式文法对由单词序列（如英语单词序列）构成的输入文本进行分析并确定其语法结构的一种过程。

语法分析器的任务主要是确定是否可以以及如何从语法的起始符号推导出输入符号串（输入文本），主要可以通过两种方式完成：   
自顶向下分析：根据形式语法规则，在语法分析树的自顶向下展开中搜索输入符号串可能的最左推导。单词按从左到右的顺序依次使用。   
自底向上分析：语法分析器从现有的输入符号串开始，尝试将其根据给定的形式语法规则进行改写，最终改写为语法的起始符号。   

一些语法分析工具总结：

English Parser   
Stanford Parser <http://nlp.stanford.edu/software/lex-parser.shtml>   
Berkeley Parser <http://nlp.cs.berkeley.edu/Main.html#Parsing>   

Dependency Parser   
CaboCha: A tool for Japanese dependency structure analysis based on cascaded chunking.   
KNP: A Japanese dependency parser that also includes some form of predicate-argument analysis.   
MaltParser: A parser based on the shift-reduce method.   
MSTParser: A tool for dependency parsing based on maximum spanning trees.   
<http://muyefeifei.com/自然语言处理常用工具及选择汇总/>   

句法分析工具比较：   
1. 复旦 NLP 是一个中文自然语言处理工具包，集成了基于转换的依存句法分析算法，并且 java 调用简单方便。另外，句法关系用中文标示，简单易用。   
2. 哈工大 NLP 是一个用 c 语言开发的中文自然语言处理工具包，集成了基于图的句法分析算法。但是需要利用 jni 编译才能利用 java 调用，不利于跨平台的应用。   
3. HanLP 作为个人开发的中文自然语言处理工具包，虽然能够进行句法分析。但是，在测试集上的效果不是很理想，可能不是很稳定。   
4. 斯坦福 NLP 作为一个强大的自然语言处理工具包，集成了多种句法分析的算法（包括最新的神经网络模型）并有相应训练的中文模型。从测试的效果来看，中文的效果不是很好，可能用的中文模型不是很正确。   
另外，在运行时间上大约都为 2ms 级别左右，在使用时时间上应该不是问题。同时，所有的工具都已经安装进行了实例测试。   
<http://cslt.riit.tsinghua.edu.cn/mediawiki/images/e/e5/句法工具分析.pdf>   


***相关连接***

 - <https://zh.wikipedia.org/wiki/生成文法>
 - <https://zh.wikipedia.org/wiki/語法分析器>

