---
layout: post
title: Density Peak Clustering
category: Algorithms
comments: true
---

# Clustering by fast search and find of density peaks

------

Cluster analysis is aimed at classifying elements into categories on the basis of their similarity. Its applications range from astronomy to bioinformatics, bibliometrics, and pattern recognition. 

This paper propose an approach based on the idea that cluster centers are characterized by a higher density than their neighbors and by a relatively large distance from points with higher densities. 

<http://science.sciencemag.org/content/344/6191/1492>

This idea forms the basis of a clustering procedure in which the number of clusters arises intuitively, outliers are automatically spotted and excluded from the analysis, and clusters are recognized regardless of their shape and of the dimensionality of the space in which they are embedded.

Several different clustering strategies have been proposed, but no consensus has been reached even on the definition of a cluster. 

In K-means and K-medoids methods, clusters are groups of data characterized by a small distance to the cluster center. An objective function, typically the sum of the distance to a set of putative cluster centers, is optimized until the best cluster centers candidates are found. However, because a data point is always assigned to the nearest center, these approaches are not able to detect nonspherical clusters.

In distribution-based algorithms, one attempts to reproduce the observed realization of data points as a mix of predefined probability distribution functions; the accuracy of such methods depends on the capability of the trial probability to represent the data.

The algorithm has its basis in the assumptions that cluster centers are surrounded by neighbors with lower local density and that they are at a relatively large distance from any points with a higher local density. For each data point i, we compute two quantities: its local density ri and its distance di from points of higher density. Both these quantities depend only on the distances dij between data points, which are assumed to satisfy the triangular inequality. The local density ri of data point i is defined as

![此处输入图片的描述][1]

dc is a cutoff distance. Basically, ri is equal to the number of points that are closer than dc to point i. The algorithm is sensitive only to the relative magnitude of ri in different points, implying that, for large data sets, the results of the analysis are robust with respect to the choice of dc.

![此处输入图片的描述][2]

di is measured by computing the minimum distance between the point i and any other point with higher density. For the point with highest density, we conventionally take di=maxj(dij). Note that di is much larger than the typical nearest neighbor distance only for points that are local or global maxima in the density. Thus, cluster centers are recognized as points for which the value of di is anomalously large.

![此处输入图片的描述][3]

The algorithm in two dimensions. (A) Point distribution. Data points are ranked in order of decreasing density. (B) Decision graph for the data in (A). Different colors correspond to different clusters.

![此处输入图片的描述][4]

Results for synthetic point distributions. (A) The probability distribution from which point distributions are drawn. The regions with lowest intensity correspond to a background uniform probability of 20%. (B and C) Point distributions for samples of 4000 and 1000 points, respectively. Points are colored according to the cluster to which they are assigned. Black points belong to the cluster halos. (D and E) The corresponding decision graphs, with the centers colored by cluster. (F) The fraction of points assigned to the incorrect cluster as a function of the sample dimension. Error bars indicate the standard error of the mean.

![此处输入图片的描述][5]

In our algorithm, we do not introduce a noise-signal cutoff. Instead, we first find for each cluster a border region, defined as the set of points assigned to that cluster but being within a distance dc from data points belonging to other clusters. They then find, for each cluster, the point of highest density within its border region. They denote its density by rb. The points of the cluster whose density is higher than rb are considered part of the cluster core (robust assignation). The others are considered part of the cluster halo (suitable to be considered as noise).

The method is robust with respect to changes in the metric that do not significantly affect the distances below dc, that is, that keep the density estimator in Eq.1 unchanged. Clearly, the distance in Eq.2 will be affected by such a change of metric, but it is easy to realize that the structure of the decision graph (in particular, the number of data points with a large value of d) is a consequence of the ranking of the density values, not of the actual distance between far away points.

This approach only requires measuring (or computing) the distance between all the pairs of data points and does not require parameterizing a probability distribution or a multidimensional density function. Therefore, its performance is not affected by the intrinsic dimensionality of the space in which the data points are embedded.

![此处输入图片的描述][6]

They also applied the approach to the Olivetti Face Database, a widespread benchmark for machine learning algorithms, with the aim of identifying, without any previous training, the number of subjects in the database. This data set poses a serious challenge to our approach because the "ideal" number of clusters (namely of distinct subjects) is comparable with the number of elements in the data set (namely of different images, 10 for each subject). This makes a reliable estimate of the densities difficult. The similarity between two images was computed by following. The density is estimated by a Gaussian kernel with variance dc=0.07. For such a small set, the density estimator is unavoidably affected by large statistical errors; thus, we assign images to a cluster following a slightly more restrictive criterion than in the preceding examples. An image is assigned to the same cluster of its nearest image with higher density only if their distance is smaller than dc. As a consequence, the images further than dc from any other image of higher density remain unassigned. 

(A) The decision graph for the first hundred images in the database. (B) The value of γi=ridi in decreasing order for the data in (A). (C) The performance of the algorithm in recognizing subjects in the full database as a function of the number of clusters: number of subjects recognized as individuals (black line), number of clusters that include more than one subject (red line), number of subjects split in more than one cluster (green), and number of images assigned to a cluster divided by 10 (purple). (D) Pictorial representation of the cluster assignations for the first 100 images. Faces with the same color belong to the same cluster, whereas gray images are not assigned to any cluster. Cluster centers are labeled with white circles.

***相关连接***

 - http://science.sciencemag.org/content/344/6191/1492

  [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_1.png
  [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_2.png
  [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_3.png
  [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_4.png
  [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_5.png
  [6]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2016-08-01-density_peak_clustering/2016-08-01-density_peak_clustering_6.png

  