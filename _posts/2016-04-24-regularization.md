---
layout: post
title: 深度学习正则的应用
category: Notes
comments: true
---

# Regularization for Deep learning

------

机器学习中如果训练数据不够，或者过训练(overtraining)的时候,，通常会导致过拟合(overfitting)。通过实验容易观测到随着训练过程的进行，模型复杂度逐渐增加，在训练集上的误差渐渐减小，而在验证集上的误差首先减小，但继续训练却反而渐渐增大，这是因为训练出来的网络过拟合了训练集，例如数据在特征长的分布，而对训练数据却不能很好的预测。通常，深度学习或神经网络的学习中也存在相同的问题。  

为了防止过拟合，通常会采用以下方法，而各个方法可以叠加使用。  

通常最常用简单的方法是L1正则或L2正则，通用的表达形式如下。  

![此处输入图片的描述][1]

其中最后一项是正则项，对L1和L2分别对应是。

![此处输入图片的描述][2]

![此处输入图片的描述][3]

这种方法可以理解为对模型的参数施加了约束，例如将目标表达式看做带有约束的最优化问题，而采用拉格朗日变换所形成的非约束优化问题。通常也可以将先验知识采用类似的约束变化方法形成正则项加入到目标表达式。

![此处输入图片的描述][4]

![此处输入图片的描述][5]

![此处输入图片的描述][6]

另一种方法称为数据集扩充(Data Augmentation)。其原理比较简单，通常会针对小数据集，对不同数据的特征进行组合、加噪等，扩充数据的丰富程度，提高模型的泛化能力。

除了L1或L2正则，神经网络里面还有一类常用的方法称为Dropout。L1、L2正则化是通过修改代价函数来实现的，而Dropout则是通过修改神经网络本身来实现的，它是在训练网络时用的一种技巧。细节解释可以参考论文Dropout: A Simple Way to Prevent Neural Networks from Overfitting。

下图是一个3层的神经网络的dropout示意图： 

![此处输入图片的描述][7]

可以这么理解，假设我们要训练上图这个网络，我们对全体神经元以概率P做采样(随机“删除”一些隐层单元，保持输入输出层不变)，只更新选出的神经元的参数。针对Dropout可以这样解释，在训练过程中相当于训练了很多个只有半数隐层单元的神经网络或“半数网络”，而随着训练的进行，大部分半数网络都可以给出正确的分类结果，减轻了少数的错误分类结果的影响，这种思想有点类似于集成学习(Ensemble Learning)。需要注意，在测试阶段，通常不用dropout，而是直接从概率的角度，对权重配以一个概率值。

To Be Continue.


***相关连接***

 - http://www.deeplearningbook.org/contents/regularization.html


  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-04-24-regularization/2016-04-24-regularization_7.png
