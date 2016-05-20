---
layout: post
title: Recursive Neutral Network
category: Notes
comments: true
---

# Recursive Neutral Network

------

### Recursive Neutral Network

结构递归神经网络（Recursive Neutral Network）是一类用结构递归的方式构建的网络，例如递归自编码机（Recursive Autoencoder or RAE），常用于在自然语言处理的神经网络分析方法中用于解析语句。

![此处输入图片的描述][1]

![此处输入图片的描述][2]

![此处输入图片的描述][3]

![此处输入图片的描述][4]

![此处输入图片的描述][5]

![此处输入图片的描述][6]

![此处输入图片的描述][7]

![此处输入图片的描述][8]

![此处输入图片的描述][9]

![此处输入图片的描述][10]

------

RAE的实现代码搜集如下。

 - Numpy   
<https://gist.github.com/ktnyt/c426a0a0da1180fd8e48>

 - Theano   
<https://github.com/shawntan/rnn-experiment/blob/master/rae.py>   
<https://blog.wtf.sg/2014/05/12/recursive-auto-encoders-example-in-theano/>

 - str2vec   
<https://github.com/pengli09/str2vec>   
str2vec is a toolkit for computing vector-space representations for variable-length phrases using recursive autoencoders (RAE)   
Reference: Peng Li, Yang Liu, Maosong Sun. Recursive Autoencoders for ITG-based Translation. Proc. of EMNLP 2013: Conference on Empirical Methods in Natural Language Processing, Seattle, Washington, USA, 2013, pp. 567-577.

 - Implementation by Socher (Matlab)

<http://www.socher.org/index.php/Main/Semi-SupervisedRecursiveAutoencodersForPredictingSentimentDistributions>

```
@inproceedings{SocherEtAl2011:RAE,
author = {Richard Socher and Jeffrey Pennington and Eric H. Huang and Andrew Y. Ng and Christopher D. Manning},
title = {{Semi-Supervised Recursive Autoencoders for Predicting Sentiment Distributions}},
booktitle = {Proceedings of the 2011 Conference on Empirical Methods in Natural Language Processing (EMNLP)}, 
year = 2011
}
```

<http://www.socher.org/index.php/Main/DynamicPoolingAndUnfoldingRecursiveAutoencodersForParaphraseDetection>

```
@incollection{SocherEtAl2011:PoolRAE,
title = {{Dynamic Pooling and Unfolding Recursive Autoencoders for Paraphrase Detection}},
author = {{Richard Socher and Eric H. Huang and Jeffrey Pennington and Andrew Y. Ng and Christopher D. Manning}},
booktitle = {{Advances in Neural Information Processing Systems 24}},
year = {2011}
}
```

------

顺便搜集了一下Deep Learning大牛们的主页或blog。

 - Geoffrey Hinton
<http://www.cs.toronto.edu/~hinton/>

 - Yann LeCun
<http://yann.lecun.com/>

 - Yoshua Bengio
<http://www.iro.umontreal.ca/~bengioy/yoshua_en/index.html>

 - Andrew Ng
<http://www.andrewng.org/>

 - Chris Manning
<http://nlp.stanford.edu/~manning/>

 - Richard Socher
<http://www.socher.org/index.php/Main/HomePage>

 - Honglak Lee
<http://web.eecs.umich.edu/~honglak/>

 - Rajat Raina
<http://ai.stanford.edu/~rajatr/research.html>


***相关连接***

 - http://www.socher.org/
 - http://nlp.stanford.edu/~socherr/thesis.pdf


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png
  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-05-21-Recursive/2016-05-21-Recursive_1.png

