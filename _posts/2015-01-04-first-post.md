---
layout: post
title: Welcome!
image: /img/hello_world.jpeg
---


Computational Biologist at Fred Hutch. 
Passionate about R and Bioconductor.  

S4 classes for assay data  
Machine-learning (tuning, benchmarking, resampling, ensemble)  
Inferential models  


## Meta analysis of multi-assay data: proof of concept
poster: Prototype meta-analysis demonstration for ImmuneSpaceR, using designated S4 objects
[https://www.bioconductor.org/help/course-materials/2017/BioC2017/DDay/LightningTalk/SessionII/ImmuneSpaceR.pdf](https://www.bioconductor.org/help/course-materials/2017/BioC2017/DDay/LightningTalk/SessionII/ImmuneSpaceR.pdf)


## Bioc2mlr
R package to bridge between Bioconductor’s S4 complex genomic data container, to mlr, a meta machine learning aggregator package. 

Bioconductor's S4 data containers for genomic assays are popular, well established data structures. Their data architecture facilitates the application of common analytical procedures and well established statistical methodologies to large assay data. They are extensible to encompass new emerging technologies and analytical methods.
However, the S4 system enforces strict constraints on the data and these constraints raise barriers for interoperability and integration with software and packages outside of Bioconductor's repository.
mlr is a comprehensive package for machine learning. It aggregates hundreds of supervised and unsupervised models and facilitates analytics such as resampling, benchmarking, tuning, and ensemble. The mlrCPO package extends mlr's pre-processing and feature engineering functionality via composable Preprocessing Operators (CPO) 'pipelines'.

Bioc2mlr is a compact utility package designed to bridge between these approaches. It deploys transformations of SummarizedExperiment and MultiAssayExperiment S4 data structures into mlr's expected format. It also implements Bioconductor's popular feature selection (filtering) methods used by limma package and others, as a CPO. The vignettes present comparisons to the MLInterfaces package, which aims to achieve similar goals, and presents workflows for popular publicly available genomic datasets such as curatedTCGAData.

Bioc2mlr:
[https://github.com/drorberel/Bioc2mlr](https://github.com/drorberel/Bioc2mlr)
