---
layout: post
title: "The Normalization Advantage for Laplacian Matrices"
author: Carol Pachar
date: 2025-12-02 
categories: [Laplacian Matrix, Spectral Graph Theory, Python]
---

## Introduction

Clustering is a data analysis technique. When looking at large data sets, clustering can help us identify groups of data that have a “similar behavior” (von Luxburg, 2007, p. 395). 
The goal of clustering is to make groups of data in which the data points inside of a group are similar to each other while datapoints from different groups are dissimilar 
to each other (von Luxburg, 2007, p. 396). Spectral clustering is a clustering algorithm that's been found to have superior advantages compared to traditional clustering algorithms such as k-means or single 
linkage (von Luxburg, 2007, p. 395). 

Next, I will introduce the mathematical objects used in spectral clustering: similarity graphs and laplacian matrices.


## Similarity Graphs
 
A similarity graph is a node-link diagram in which nodes are laid out according to their similarity. The more similar two nodes are to each other, the closer they appear in the diagram (Hammad et al., 2020, p. 36). 
Often, similarity is a function of the edges, that is, strongly connected nodes are attracted to each other (Hammad et al., 2020, p. 36). There are different variants of similarity graphs, which differ in what nodes and edges represent and how similarity is defined (Hammad et al., 2020, p. 36).

Below is a classic example of a similarity graph that demonstrates Zachary's karate club. 

<img width="597" height="557" alt="Zachary&#39;s_karate_club (1)" src="https://github.com/user-attachments/assets/820b33d0-26c6-4a2f-ad34-da9fe203d561" />

"Zachary's karate club" by CuneytAkcora is licensed under CC BY-SA 4.0.


## A Laplacian Matrix
The main tool of spectral clustering are laplacian matrices. There is a whole field that's dedicated to studying these matrices, called spectral graph theory (von Luxburg, 2007, p. 397). 

A laplacian matrix is defined as a matrix that is diagnolly dominant, contains non-positive off-diagnol entries, and is symmetric (Spielman, 2017, 29:59).

## A Real-World Application of Laplacian Matrices 
Using the graph shown below, let each node represent a gene. Say we initially only know the values of gene A and gene D (gene A has a value of 0 and gene D has a value of 10). We want to guess the values of the function at other nodes. We know that some of these genes interact with each other. A scientist might be trying to figure out which genes might be implicated in a disease, like diabetes. For this example, let 0 mean that the gene is not implicated in diabetes while 10 means that the gene is certain to be implicated in diabetes. By using the known values of gene A and gene D, we can use the Laplacian matrix to calculate the other gene’s probability of being related to the disease (Spielman, 2017, 4:54-8:19) (note that the scale Spielman uses in the video lecture is from 0 to 1 with 0 meaning that the gene is not implicated in diabetes while 1 means that the gene is certain to be implicated in diabetes). 

<img width="512" height="427" alt="512px-Graph-weighting-function svg" src="https://github.com/user-attachments/assets/6c4741f6-9cb7-44ee-b4ad-d3b91cb7ee35" />

"Graph-weighting-function" by Rainhard Findling is licensed under CC BY-SA 3.0.


## Which Matrix is Better to Use?
There are two types of laplacian matrices: the Unnormalized Laplacian matrix and the Normalized Laplacian matrix (von Luxburg, 2007, p. 397-398). In literature review, when an author says that they're using a laplacian matrix, it's usually one of these matrices (von Luxburg, 2007, p. 397). 

This brings us to the question: which matrix is better to use for spectral clustering?  

To answer this question, I will evaluate each matrix in terms of their clustering performance. In other words, I will be looking at the adjusted rand index and the runtime. 

In order to evaluate each matrix's performance when used in spectral clustering, I've applied a spectral clustering function on two benchmark graph datasets: Zachary's Karate Club and a 
Stochastic Block Model. My approach involved using linear algebra to implement and compare the Unnormalized Laplacian and the Symmetrically Normalized Laplacian. For each matrix, 
the $k$ smallest non-zero eigenvectors (using numpy.linalg.eigh) was computed, discarding the trivial first vector, to create the feature embedding. This embedding was then clustered using 
the $k$-means algorithm. 

## Result

The results below are derived from the test runs found in Karate_Club_Test.ipynb and Stochastic_Block_Model.ipynb. 

1. Zachary's Karate Club

| Matrix | ARI (Clustering Quality)    | Runtime (seconds)    |
| :---:   | :---: | :---: |
| Unnormalized | 0.0000   | 0.0728   |
| Normalized | 0.0216   | 0.0039   |


2. Stochastic Block Model (SBM) 

| Matrix | ARI (Clustering Quality)    | Runtime (seconds)    |
| :---:   | :---: | :---: |
| Unnormalized | 0.0015   | 0.0602   |
| Normalized | 0.6789   | 0.0037   |


## Conclusion 

The main finding was that the choice of Laplacian matrix has a massive impact on clustering performance.

The quantative clustering performance for each matrix is clear: the Normalized Laplacian matrix outperformed the Unnormalized Laplacian matrix, in terms of clustering quality, when applied in both data sets. For example, under the SBM data set, the Normalized Laplacian achieved an ARI of 0.6789 compared to the near-random performance of the Unnormalized Laplacian, which had an ARI of 0.0015. In terms of runtime, the Normalized Laplacian offered a significantly faster runtime (up to 18x faster in the Karate Club trial) for the combined spectral decomposition and $k$-means steps across both datasets.

In short, the Normalized Laplacian matrix proved to be the better choice of matrices for the spectral clustering algorithm when applied in Zachary's Karate Club and in the 
Stochastic Block Model datasets. 

I can definitely see myself in the future looking back at this project. After graduation, I hope to land a career in data science, which values probability techniques. The goal of this project was to help me deepen my understanding of one of these techniques, utilizing a laplacian matrix. Both the literature review and the experiment that I've done have definitely helped me get my feet in this topic. In the future, for my research (whether it be over the summer or for my capstone), I hope I can take this project a step further by applying it on larger data sets, or even refine the code that I've made. 


## Citations

CuneytAkcora. (2017). Zachary’s karate club [Digital Image]. Wikimedia Commons.
    https://commons.wikimedia.org/w/index.php?curid=57742757

Findling, R. (2010). Graph-weighting-function [Digital image]. Wikimedia Commons.
    https://commons.wikimedia.org/w/index.php?curid=11806011

Hammad, M., Basit, H. A., Jarzabek, S., & Koschke, R. (2020). A systematic mapping study of 
    clone visualization. Computer Science Review, 37, Article 100266.
    https://doi.org/10.1016/j.cosrev.2020.100266

Spielman, D. A. (2017, April 17). The Laplacian Matrices of Graphs: Algorithms and Applications 
    [Video file]. Retrieved from  
    http://www.youtube.com/watch?v=EjpMnU79neo

von Luxburg, U. (2007). A tutorial on spectral clustering. Statistics and Computing, 
    17(4), 395–416. https://doi.org/10.1007/s11222-007-9033-z
