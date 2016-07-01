---
layout: post
title: Clustering Uncertain Data
category: Algorithms
comments: true
---

# Clustering Uncertain Data

------

### Clustering Uncertain Data

Clustering on uncertain data, one of the essential tasks in mining uncertain data, posts significant challenges on both modeling similarity between uncertain objects and developing efficient computational methods.

The previous methods extend traditional partitioning clustering methods like k-means and density-based clustering methods like DBSCAN to uncertain data, thus rely on geometric distances between objects. Such methods cannot handle uncertain objects that are geometrically indistinguishable, such as products with the same mean but very different variances in customer ratings. Surprisingly, probability distributions, which are essential characteristics of uncertain objects, have not been considered in measuring similarity between uncertain objects.

Clustering Uncertain Data Based on Probability Distribution Similarity

<https://www.computer.org/csdl/trans/tk/2013/04/ttk2013040751.pdf>

 - use the well known Kullback- Leibler divergence to measure similarity between uncertain objects in both the continuous and discrete cases, and integrate it into partitioning and density-based clustering methods to cluster uncertain objects.
 - estimate KL divergence in the continuous case by kernel density estimation and employ the fast Gauss transform technique to further speed up the computation

Generally, an uncertain data object can be represented by a probability distribution. One challenge in this clustering task is that we need to consider the similarity between cameras not only in terms of their score values, but also their score distributions.

## Limitations of Existing Work

As an object in a certain data set is a single point, the distribution regarding the object itself is not considered in traditional clustering algorithms. Thus, the studies that extended traditional algorithms to cluster uncertain data are limited to using geometric distance-based similarity measures, and cannot capture the difference between uncertain objects with different distributions.

### Partitioning clustering approaches

This method extend the k-means method with the use of the ex- pected distance to measure the similarity between two uncertain objects. As every object has the same center, the ex- pected distance-based approaches cannot distinguish the two sets of objects having different distributions.

### Density-based clustering approaches

The basic idea behind the algorithms does not change – objects in geometrically dense regions are grouped together as clusters and clusters are separated by sparse regions. However, in our case, objects heavily overlap. There is no clear sparse regions to separate objects into clus- ters. Therefore, the density-based approaches cannot work well.

### Possible world approaches

A set of possible worlds are sampled from an uncertain data set. Each possible world consists of an instance from each object. Clustering is conducted individually on each possible world and the final clustering is obtained by aggregating the clustering results on all possible worlds into a single global clustering. The goal is to minimize the sum of the difference between the global clustering and the clustering of every possible world. The possible world approaches often cannot provide a stable and meaningful clustering result at the object level, not to mention that it is computationally infeasible due to the exponential number of possible worlds.

## Kullback-Leibler divergence

In information theory, the similarity between two distributions can be measured by the Kullback-Leibler divergence (KL divergence for short, also known as information entropy or relative entropy).

![此处输入图片的描述][1]

The distribution difference cannot be captured by geometric distances.The two objects (each one is represented by a set of sampled points) have different geometric locations. Their probability density functions over the entire data space are different and the difference can be captured by KL divergence. Although the geometric locations of the two objects are heavily overlapping, they have different distributions (one is uniform and the other is Gaussian). The difference between their distributions can also be discovered by KL divergence, but cannot be captured by the existing methods.

If the domain is discrete (e.g., categorical) with a finite or countably infinite number of values, the object is a discrete random variable and its probability distribution is described by a probability mass function (pmf for short). Otherwise if the domain is continuous with a continuous range of values, the object is a continuous random variable and its probability distribu- tion is described by a probability density function (pdf for short).

For discrete domains, the probability mass function of an uncertain object can be directly estimated by normalizing the number of observations against the size of the sample. Formally, the pmf of object P is

![此处输入图片的描述][2]

For continuous domains, we estimate the probabil- ity density function of an uncertain object by kernel density estimation. In 1-dimensional case, the density estimator is

![此处输入图片的描述][3]

For the d-dimensional (d ≥ 2) case, the kernel function is the product of d Gaussian functions, each with its own bandwidth hj (1 ≤ j ≤ d), and the density estimator is

![此处输入图片的描述][4]

In general, KL divergence between two probability distributions is defined as follows,

![此处输入图片的描述][5]

![此处输入图片的描述][6]

In both discrete and continuous cases, KL diver- gence is defined only in the case where for any x in domain D if f(x) > 0 then g(x) > 0. Note that, KL divergence is not symmetric in general, that is, D(f ∥g) ̸= D(g∥f).

### Using KL Divergence as Similarity

KL divergence is always non-negative, and satisfies Gibbs’ inequality. That is, D(P ∥Q) ≥ 0 with equality only if P = Q. Therefore, the smaller the KL diver- gence, the more similar the two uncertain objects.

model every uncertain object to be clustered as a random variable in the same bounded domain D and represent it with a sample of i.i.d. observations.

### Clustering Algorithms

#### Partitioning Clustering Methods

Using KL divergence as similarity, a partitioning clustering method tries to partition objects into k clusters and chooses the best k representatives, one for each cluster, to minimize the total KL divergence as below,

![此处输入图片的描述][7]

k-means and k-medoids are two classi- cal partitioning methods. we adopt the k-medoids method to demonstrate the performance of partitioning clustering methods using KL divergence to cluster uncertain objects.

 - Building Phase: In the building phase, the uncertain k-medoids method obtains an initial clustering by selecting k representatives one after another. 
 - Swapping Phase: In the swapping phase, the uncertain k-medoids method iteratively improves the clustering by swapping a non-representative object with the representative to which it is assigned. 

The randomized k-medoids method, instead of finding the optimal non-representative object for swapping, randomly selects a non-representative object for swapping if the clustering quality can be improved.

We follow the simulated annealing technique to prevent the method from being stuck at a local optimal result.

#### Density-Based Clustering Methods

The uncertain DBSCAN method finds dense regions through core objects whose ε-neighborhood contains at least μ objects.

Initially, every core object forms a cluster. Two clusters are merged together if a core object of one cluster is density-reachable from a core object of the other cluster. A non-core object is assigned to the closest core object if it is direct density reachable from this core object. The algorithm iteratively examines objects in the data set until no new object can be added to any cluster.

Note that direct density-reachability described above is an asymmetric relationship. This is not caused by the asymmetry of KL divergence.

### Boosting Computation

The main idea of approximating the sum of a series of Gaussian functions is to make use of the fact that the probability density of the Gaussian function decays exponentially and is negligible outside a certain distance to the center of the Gaussian. The fast Gauss transform is one of the best techniques to do the job. It reduces the computational complexity from O(N × M) to O(N + M) using a divide-and-conquer strategy and combined with the manipulation of Her- mite polynomial expansions and Taylor series.

The improved fast Gauss transform takes advan- tages of the exponential decay of the Gaussian. Another key technique is to expand the sum of Gaussian functions into a multivariate Taylor expansion and truncate the series after degree p, where p is chosen to be sufficiently large such that the bounded error is no more than the precision parameter ε.

 - Step 1:Approximate the sample of C by clustering.
 - Step 2:Choose parameter p for truncating Taylor expansion.
 - Step 3:Compute the coefficients of the Taylor expansion.
 - Step 4:Compute the sum of approximated Gaussian functions.

***相关连接***

 - https://www.computer.org/csdl/trans/tk/2013/04/ttk2013040751.pdf

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_6.png
  [7]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-06-27-clustering_uncertain_data/2016-06-27-clustering_uncertain_data_7.png
