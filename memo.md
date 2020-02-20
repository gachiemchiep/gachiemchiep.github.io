
# Metrics learning

https://towardsdatascience.com/importance-of-distance-metrics-in-machine-learning-modelling-e51395ffe60d
-> not good
-> distance metrics: 
  1. Minkowski Distance
  2. Manhattan Distance
  3. Euclidean Distance
  4. Cosine Distance
  5. Mahalanobis Distance

https://en.wikipedia.org/wiki/Similarity_learning
-> wikipedia
 It has applications in ranking, in recommendation systems, visual identity tracking, face verification, and speaker verification.
 
 4 types of similarity learning
   Regression similarity learning
   Classification similarity learning
   Ranking similarity learning
   Locality sensitive hashing (LSH)[2]
 -> goals are different so the distance will also be different
 
 
 Deep Metric Learning(以下DML)
 [Deep Metric Learning 入門](https://qiita.com/gesogeso/items/547079f967d9bbf9aca8)
 -> very good articles
   -> よく間違えられる事項として, 「DMLはクラス分類はできないのか？」という質問が来ます. フィッシャーの線形判別分析を思い出してください. これは, 2クラスのクラス内分散が最小に, クラス間分散が最大となるような「境界線を引くアルゴリズム」です. 一方で, DMLは埋め込み空間上でクラス内分散が小さく, クラス間分散が大きくなるような「変換を学習させるアルゴリズム」です. したがって, DMLは分類問題とは根本的に異なります.
   -> 新しい記事について、いくつ記載されている
   
   [https://copypaste-ds.hatenablog.com/entry/2019/03/01/164155](Metric Learning 入門)
   -> easy to understand, 多くの実験結果を載せています
   -＞　10日間だけの成果、なかなか頭が良い人
   
   [http://researchers.lille.inria.fr/abellet/talks/metric_learning_tutorial_CIL.pdf](metrics learning tutorial)
   -> a lot of maths

    [http://webia.lip6.fr/~cord/pdfs/courses/2019RIVCourse2.pdf](aaa)
    -> good one too

# Personally thinking

## Can we use object proposal algorithm (such as BING, DeepBox) to perform board segmentation 
-> huhm, this is nonsense anyway

## One shot learning for semantic segmentation or even object detection

Read the slide here : https://drive.google.com/drive/folders/1sTjR75hgDDML2owb9erRdnKVLRvuluo4
https://github.com/timy90022/One-Shot-Object-Detection

one-shot learning : 
  Matching Networks
    Input: 
      a. anchor : base image
      b. other test image
  One-Shot : object detection 
    Output : which image is match

  One-Shot Instance Segmentation
    Given a query image and a reference image showing an object of a novel category, we seek to detect and segment all instances of the corresponding category (in the image above ‘person’ on the left, ‘car’ on the right). 
    https://github.com/bethgelab/siamese-mask-rcnn : 2018
    https://github.com/timy90022/One-Shot-Object-Detection : 2019
    
    -> very promising , but it's hard to implemented

## One shot object detection

Found the following 2 promissing approach

1. RepMet: Representative-based metric learning for classification and one-shot object detection
   https://github.com/jshtok/RepMet : 2018
2. One-Shot Object Detection with Co-Attention and Co-Excitation   
   https://github.com/timy90022/One-Shot-Object-Detection : 2019
   
   
Let's compare the 1. and 2.
1. DML : only learn the baseline
2. Re-write the entire network architecture

-> We will use 2 for this task

apple has very great turi examples at: https://apple.github.io/turicreate/docs/userguide/one_shot_object_detection/
  -> if we need to re-train an one-shot-object-detection then this is a good resource
  -> don't know which algorithm are they using 
   
   
Other
1. https://github.com/bingykang/Fewshot_Detection 
   -> Few-shot Object Detection via Feature Reweighting
  -> few-shot problems : have the base model, then input some random images, 
    -> we-erighting -> the model will have a very good feature nows
    -> very strange, maybe we need to read this deeper
