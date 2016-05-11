---
layout: post
title: 依存语法
category: Notes
comments: true
---

# 依存语法

------

### 依存语法

与短语结构语法比较起来，依存语法没有词组这个层次，每一个结点都与句子中的单词相对应，它能直接处理句子中词与词之间的关系，而结点数目大大减少了，便于直接标注词性，具有简明清晰的长处。特别在语料库文本的自动标注中，使用起来比短语结构语法方便。

一般而言，短语结构语法是与依存语法等价的。因此，如果我们在短语结构分析之后得到了短语结构树，可以自动地把这样的短语结构树转换为依存树。

如果在短语结构树中，确定了结点之间的依存关系，把处于支配地位的词叫做主词，处于依存地位的词叫做从词，那么，就可以把短语结构树转化为依存树，转换的步骤是：

 - 从叶子结点开始，首先把表示具体单词的结点归结到表示词类的结点上；   
 - 然后，自底向上把主词归结到父结点上；   
 - 最后再把全句的中心主词归结到根结点上。 

通过这样的步骤，便可以得到与短语结构树等价的依存树。   
由此可见，依存语法与短语结构语法具有等价性。通过有穷的步骤，我们不难实现短语结构语法和依存语法之间的相互转化。   

在20世纪70年代，Robinson提出依存语法中关于依存关系的四条公理，在处理中文信息的研究中，中国学者提出了依存关系的第五条公理，如下：

 - 一个句子中只有一个成分是独立的；   
 - 其它成分直接依存于某一成分；   
 - 任何一个成分都不能依存与两个或两个以上的成分；   
 - 如果A成分直接依存于B成分，而C成分在句中位于A和B之间，那么C或者直接依存于B，或者直接依存于A和B之间的某一成分；   
 - 中心成分左右两面的其它成分相互不发生关系。   

句子成分间相互支配与被支配、依存与被依存的现象普遍存在于汉语的词汇（合成语）、短语、单句、复合直到句群的各级能够独立运用的语言单位之中，这一特点为依存关系的普遍性，依存句法分析可以反映出句子各成分之间的语义修饰关系，它可以获得长距离的搭配信息，并与句子成分的物理位置无关。

句法分析工具总结：

 * 复旦NLP <https://github.com/xpqiu/fnlp>

```
package org.fnlp.demo.nlp;
import org.fnlp.nlp.cn.tag.POSTagger;
import org.fnlp.nlp.parser.dep.DependencyTree;
import org.fnlp.nlp.parser.dep.JointParser;

public class DepParser {
	private static JointParser parser;

	public static void main(String[] args) throws Exception {
		parser = new JointParser("../models/dep.m");
		
		System.out.println("得到支持的依存关系类型集合");
		System.out.println(parser.getSupportedTypes());
		
		String word = "中国进出口银行与中国银行加强合作。";
		test(word);

	}

	private static void test(String word) throws Exception {		
		POSTagger tag = new POSTagger("../models/seg.m","../models/pos.m");
		String[][] s = tag.tag2Array(word);
		try {
			DependencyTree tree = parser.parse2T(s[0],s[1]);
			System.out.println(tree.toString());
			String stree = parser.parse2String(s[0],s[1],true);
			System.out.println(stree);
		} catch (Exception e) {			
			e.printStackTrace();
		}
	}
}
```

 * 哈工大NLP <https://github.com/HIT-SCIR/ltp/>

```
#include <iostream>
#include <vector>

#include "ltp/parser_dll.h"

int main(int argc, char * argv[]) {
  if (argc < 2) {
    return -1;
  }

  void * engine = parser_create_parser(argv[1]);
  if (!engine) {
    return -1;
  }

  std::vector<std::string> words;
  std::vector<std::string> postags;

  words.push_back("一把手");  postags.push_back("n");
  words.push_back("亲自");    postags.push_back("d");
  words.push_back("过问");    postags.push_back("v");
  words.push_back("。");      postags.push_back("wp");

  std::vector<int>      heads;
  std::vector<std::string>  deprels;

  parser_parse(engine, words, postags, heads, deprels);

  for (int i = 0; i < heads.size(); ++ i) {
    std::cout << words[i] << "\t" << postags[i] << "\t"
              << heads[i] << "\t" << deprels[i] << std::endl;
  }

  parser_release_parser(engine);
  return 0;
}
```

 * HanLP <https://github.com/hankcs/HanLP>

```
package com.hankcs.demo;

import com.hankcs.hanlp.HanLP;
import com.hankcs.hanlp.corpus.dependency.CoNll.CoNLLSentence;
import com.hankcs.hanlp.corpus.dependency.CoNll.CoNLLWord;

public class DemoDependencyParser
{
    public static void main(String[] args)
    {
        CoNLLSentence sentence = HanLP.parseDependency("徐先生还具体帮助他确定了把画雄鹰、松鼠和麻雀作为主攻目标。");
        System.out.println(sentence);
        // 可以方便地遍历它
        for (CoNLLWord word : sentence)
        {
            System.out.printf("%s --(%s)--> %s\n", word.LEMMA, word.DEPREL, word.HEAD.LEMMA);
        }
        // 也可以直接拿到数组，任意顺序或逆序遍历
        CoNLLWord[] wordArray = sentence.getWordArray();
        for (int i = wordArray.length - 1; i >= 0; i--)
        {
            CoNLLWord word = wordArray[i];
            System.out.printf("%s --(%s)--> %s\n", word.LEMMA, word.DEPREL, word.HEAD.LEMMA);
        }
        // 还可以直接遍历子树，从某棵子树的某个节点一路遍历到虚根
        CoNLLWord head = wordArray[12];
        while ((head = head.HEAD) != null)
        {
            if (head == CoNLLWord.ROOT) System.out.println(head.LEMMA);
            else System.out.printf("%s --(%s)--> ", head.LEMMA, head.DEPREL);
        }
    }
}
```

 * 斯坦福 NLP <http://nlp.stanford.edu/software/lex-parser.shtml>

```
package edu.stanford.nlp.parser.nndep.demo; 
import edu.stanford.nlp.util.logging.Redwood;

import edu.stanford.nlp.ling.HasWord;
import edu.stanford.nlp.ling.TaggedWord;
import edu.stanford.nlp.parser.nndep.DependencyParser;
import edu.stanford.nlp.process.DocumentPreprocessor;
import edu.stanford.nlp.tagger.maxent.MaxentTagger;
import edu.stanford.nlp.trees.GrammaticalStructure;

import java.io.StringReader;
import java.util.List;

public class DependencyParserDemo  {

  /** A logger for this class */
  private static Redwood.RedwoodChannels log = Redwood.channels(DependencyParserDemo.class);
  public static void main(String[] args) {
    String modelPath = DependencyParser.DEFAULT_MODEL;
    String taggerPath = "edu/stanford/nlp/models/pos-tagger/english-left3words/english-left3words-distsim.tagger";

    for (int argIndex = 0; argIndex < args.length; ) {
      switch (args[argIndex]) {
        case "-tagger":
          taggerPath = args[argIndex + 1];
          argIndex += 2;
          break;
        case "-model":
          modelPath = args[argIndex + 1];
          argIndex += 2;
          break;
        default:
          throw new RuntimeException("Unknown argument " + args[argIndex]);
      }
    }

    String text = "I can almost always tell when movies use fake dinosaurs.";

    MaxentTagger tagger = new MaxentTagger(taggerPath);
    DependencyParser parser = DependencyParser.loadFromModelFile(modelPath);

    DocumentPreprocessor tokenizer = new DocumentPreprocessor(new StringReader(text));
    for (List<HasWord> sentence : tokenizer) {
      List<TaggedWord> tagged = tagger.tagSentence(sentence);
      GrammaticalStructure gs = parser.predict(tagged);

      // Print typed dependencies
      log.info(gs);
    }
  }
}
```


### 语义角色标注

语义角色标注技术按照现代语法知识将词语序列分组，并按照语义角色对它们进行分类，它体现句子基本意思，该方法不对整个句子做详细的语义分析，只是标注句子中给定谓词（动词、名词等）的语义角色（参数），从而是计算机对语句有一个“浅层”的理解，就是对于给定句子中的每个谓词（动词、名词等）分析出其中的相应语义成分，并作相应的语义标记，如施事、受事、工具或附加语等。语义角色标注是浅层语义分析的一种实现方式，它不对整个句子进行详细的语义分析，实质是在句子级别进行浅层的语义分析，具有分析任务定义明确，便于评价等优点。

目前研究比较热门的技术有：基于依存关系的语义角色标注、基于特征向量的语义角色标注等。

 - 基于依存句法关系的语义角色标注   
	通过依存关系，依存树可以直接对与谓词节点相连的语义角色结构进行编码。   
 - 基于特征向量的语义角色标注   
	比较文本信息中不同特点和特征，以基本的特征和语料数据资源为基础，筛选便于识别和分类的特征进行语义角色标注。
 - 基于最大熵分类器的语义角色标注 
	预测了全部能够在句法分析树中找到匹配成分的角色后，采用简单的后处理规则去识别那些找不到匹配成分的角色。
 - 基于特征向量的语义角色标注   
	可以通过计算核函数隐式达到降维，降低时间和空间复杂性，实现语义角色标注技术。
 - 基于条件随机场的语义角色标注
 	它以浅层句法分析为基础，把短语或命名实体作为标注的基本单元，将条件随机场模型用于句子中谓词的语义角色标注。


***相关连接***

 - <http://stp.lingfil.uu.se/~nivre/docs/ACLslides.pdf>
 - <http://www.paper.edu.cn/download/downPaper/201404-88>
 
