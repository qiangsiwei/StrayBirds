---
layout: post
title: Semantic Role Labeling
category: Notes
comments: true
---

# Semantic Role Labeling

------

### Semantic Role Labeling

Semantic roles are utilized to find concepts automatically and assure their meaningfulness. Semantic role labeling is a research problem which finds in a given sentence the predicates and their arguments (identification), and further labels the semantic relationship between predicates and arguments, that is, their semantic roles (classification).

Gildea and Jurafsky in 2002
In their approach, they emphasize the selection of appropriate lexical and syntactical features for SRL, the use of statistical classifiers and their combinations, and ways to handle data sparseness.

Augmenting and/or altering the feature set and attempting different ways to handle data sparseness.

![此处输入图片的描述][1]

Features includes head word related features, target word related features, grammar related features, and semantic type related features.

Suppose for any given predicate P in a sentence, the system has identified the three potential arguments A1, A2, and A3 of the predicate, the semantic roles of arguments may depend on each other(argument interdependence).

All the surrounding arguments’ predicted labels are used with window size [-∞,∞]. This also conforms to the rule that when a role is taken by the other argu- ment, it is less likely that the current argument is of the same role.

![此处输入图片的描述][2]

In layer 1 the baseline system is used to predict the labels for identified nodes. Then in layer 2, these predicted labels of all surrounding argu- ments (in this example, A1 and A3) together with other features of the current node (A2) are used to predict the label of the current node.

![此处输入图片的描述][3]

The concepts are formulated by concept templates designed according to Propbank SRL labels. These role combinations serve as templates which can capture a complete and important piece of information described in one sentence to form a concept.

![此处输入图片的描述][4]

***相关连接***

 - http://www.aclweb.org/anthology/P15-4009

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-27-semantic_role_labeling/2016-05-27-semantic_role_labeling_4.png
