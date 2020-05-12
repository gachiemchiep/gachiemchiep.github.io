---
title: "Deep metrics learning summary"
excerpt: "Personal note when about deep metrics learning."
categories: 
  - Machine Learning
tags: 
  - Deep metric learning
comments: true
toc: true
support: true
published: true
order: 9
author: vugia.truong
---

# 1. Summary

Metrics learning is a learning method that aims at learning how similar or related two input objects are. 
It has applications in ranking, in recommendation systems, visual identity tracking, face verification, and speaker verification.
Each object can be considered as a feature vector inside embedding space. By learning, the distance between similar objects' feature 
vector will become closer. In opposite, the distance between dissimilar objects' feature vector will become further.

The similarity value (how similar) of input objects are calculated as follows. 
At first, *feature vectors* for each object (image, sound, text, etc) are extracted. 
Then, the distance (Euclid, Coisine, Manhattance, etc) between features vectors are calculated. 
This distance is the similarity value for 2 input objects

By using Deep Neural Network to construct a non-linear feature extractor for metrics learning, 
we will have "Deep Metrics Learning" (DML) methods.

![Summary 1](/assets/images/ml/metrics_learning_001.png)

After learning, the face image of same people will become closer while the face image of different people will become further.

![Summary 2](/assets/images/ml/metrics_learning_002.png)

# 2. Usage

DML can be used for the following task

* image searching
* verification
* identification
* clustering
* Anomaly detection
* few-shot-learning

## 2.1 Image searching

Image searching = find the similar image with *query image* from image database.

![Summary 2](/assets/images/ml/metrics_learning_003.png)

![Summary 2](/assets/images/ml/metrics_learning_004.png)

The procedure is quite simple. 

1. Extract feature vector for all image inside database
2. Extract feature vector for query image
3. Calculate the distance

## 2.2 Verification

Verification = checking whether registered image and query image matches

![Verification](/assets/images/ml/metrics_learning_005.png)

Procedure

1. Extract feature vector for registered image and query image
2. Calculate distance between 2 vectors
3. Compare with threshold value

## 2.3 Identification

Identification = who is this people ?

![Identification](/assets/images/ml/metrics_learning_007.png)

## 2.4 Clustering

DML become the feature extractor

## 2.5 Anomany detection

Same as verification

## 2.6 few-shot learning

zero-shot object detector = train object detector to detect object never seen before

![zero-shot object detector](/assets/images/ml/metrics_learning_008.png)

one-shot object detector = template matching

![one-shot object detector](/assets/images/ml/metrics_learning_009.png)

# 3. DML methods

Siamese network and Triplet network are standard learning methods for DML. 

See  [https://omoindrot.github.io/triplet-loss](Triplet Loss and Online Triplet Mining in TensorFlow) for more detail about triplet and siamese net

For newer methods, see [https://github.com/ifeherva/DMLPlayground](https://github.com/ifeherva/DMLPlayground)

TODO: more detail here

# Reference

1. [http://webia.lip6.fr/~cord/pdfs/courses/2019RIVCourse2.pdf](http://webia.lip6.fr/~cord/pdfs/courses/2019RIVCourse2.pdf)
2. [http://researchers.lille.inria.fr/abellet/talks/metric_learning_tutorial_CIL.pdf](http://researchers.lille.inria.fr/abellet/talks/metric_learning_tutorial_CIL.pdf : a lot of math)
3. [https://omoindrot.github.io/triplet-loss](Triplet Loss and Online Triplet Mining in TensorFlow)
4. [https://qiita.com/gesogeso/items/547079f967d9bbf9aca8](Deep Metric Learning 入門)
5. [https://github.com/ifeherva/DMLPlayground](https://github.com/ifeherva/DMLPlayground)
6. [https://cpp-learning.com/metric-learning/#KISSMEKeepItSimple_andStraightforwardMEtric](KISSMEKeepItSimple_andStraightforwardMEtric)
7. [https://copypaste-ds.hatenablog.com/entry/2019/03/01/164155](Metric Learning 入門)