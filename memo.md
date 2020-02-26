
# Metrics learning

TODO : more detail
   

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
