
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

# few-shot-learning

[https://blog.floydhub.com/n-shot-learning/](https://blog.floydhub.com/n-shot-learning/)


   N-shot learning = N sample for training
   few-shot learning : few = 1~5
    -> zero shot learning
    -> one shot learning
    -> few-shot learning

  training set = support set
    -> K classes, N examples per class
  testing set = query set
    -> K classes, a lot of example per class

  -> reduce the size of training set as small as possible
  -> don't know why they changed the name

  sub categories: 

    zero-shot learning : do the task on unseed object
    one-shot learning : do the task while using only 1 samples per class to learn
      -> take the advantage of siamese (metrics learning)
    few-shot learning : do the task while using only few samples per class to learn

  How to measure performance: n-shot, k-way (n-example, k-class)



  -> let's dive a little bit

  Zero-shot learning
    Learning to Compare: Relation Network for Few-Shot Learning
    Learning Deep Representations of Fine-Grained Visual Descriptions
    Improving zero-shot learning by mitigating the hubness problem

  One-Shot Learning : siamese network , matching networks
    Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks
    One-shot Learning with Memory-Augmented Neural Networks
    Prototypical Networks for Few-shot Learning

  Few-Shot learning
    -> just extend version of one-shot learning

-> famouse network : prototypical networks
  -> learn the mapping of an image in metric space (not vector space).
  -> metrics space : does not has origin
  -> vector space : has origin

  -> now understand what is few-shots learning

[https://towardsdatascience.com/advances-in-few-shot-learning-a-guided-tour-36bc10a68b77](https://towardsdatascience.com/advances-in-few-shot-learning-a-guided-tour-36bc10a68b77)


---> bat dau khong hieu van de roi, doc lai cho ro nao

# meta-learning

[https://lilianweng.github.io/lil-log/2018/11/30/meta-learning.html](Meta-Learning: Learning to Learn Fast)
  -> very good writing

[https://www.sicara.ai/blog/2019-07-30-image-classification-few-shot-meta-learning](Few-Shot Image Classification with Meta-Learning)


[https://en.wikipedia.org/wiki/Meta_learning_(computer_science)](wikipedia)
  -> a good warm up

meta-learning : learning to learn
   learning new concepts and skills fast with a few training examples? That’s essentially what meta-learning aims to solve.

  A classifier trained on non-cat images can tell whether a given image contains a cat after seeing a handful of cat pictures.
  A game bot is able to quickly master a new game.
  A mini robot completes the desired task on an uphill surface during test even through it was only trained in a flat surface environment.

  -> common approaches

  1. Model-Based
    Model-based meta-learning models updates its parameters rapidly with a few training steps, which can be achieved by its internal architecture or controlled by another meta-learner model
      -> Memory-Augmented Neural Networks
      -> Meta Networks
  2. Metrics-Based (metrics learning)
    The core idea in metric-based meta-learning is similar to nearest neighbors algorithms, which weight is generated by a kernel function. It aims to learn a metric or distance function over objects. The notion of a good metric is problem-dependent. It should represent the relationship between inputs in the task space and facilitate problem solving[
      -> Siamese network
      -> Matching Network
      -> Relation network
      -> Prototypical Networks
  3. Optimization-Based
    What optimization-based approach meta-learning algorithms intend for is to adjust the optimization algorithm so that the model can be good at learning with a few examples[7].

      -> LSTM Meta-Learner
      -> Temporal Discreteness
      -> Reptile




Few-shot object recognition became a hot topic recently (from 4 few-shot papers in CVPR18 to around 20 in CVPR19). The usual setup is that you have categories with many examples you can use at training time; then at test time, you are given novel categories (usually 5) with only a few examples per category (usually 1 or 5; called “support-set”) and query images from the same categories.
Next, I’ll try to break the few-shot methods to different families. Although these families are not well defined and many methods belong to more than one family.
“Older” suggested methods were based on metric learning, where the objective is to learn a mapping from images to embedding space in which images from the same category are closed together and from different categories are far apart. Hoping that it will hold true for unseen categories.
Following that, came meta-learning methods. These are models which are conditioned on the current task, so a different classifier is used as a function of the support-set. The idea is to find model hyper-parameters and parameters such that it will be easy to adapt to a new task without over-fitting to the few shots available.
In parallel, data augmentation methods are also very popular. The idea is to learn data augmentation so we can generate more examples out of the few examples available.
Finally, semantics-based methods are on the rise. It is inspired by zero-shot learning where classification is done based solely on the category name, textual descriptions, or attributes. Those extra semantic ques can also be of help when visual examples are scarce.


# Few Shot Semantic Segmentation Papers 


One-Shot Learning for Semantic Segmentation	
  -> 2017

[https://github.com/xiaomengyc/Few-Shot-Semantic-Segmentation-Papers](https://github.com/xiaomengyc/Few-Shot-Semantic-Segmentation-Papers)


# Incremental learning (continuous learning)

[https://github.com/xialeiliu/Awesome-Incremental-Learning](https://github.com/xialeiliu/Awesome-Incremental-Learning)
[https://wiki.continualai.org/](https://wiki.continualai.org/)
  -> wiki, huhm

  Continual Learning (CL) is built on the idea of learning continuously and adaptively about the external world and enabling the autonomous incremental development of ever more complex skills and knowledge.

  In the context of Machine Learning it means being able to smoothly update the prediction model to take into account different tasks and data distributions but still being able to re-use and retain useful knowledge and skills during time.

# A Primer in BERTology: What we know about how BERT works

https://arxiv.org/pdf/2002.12327.pdf


# Camera calibration

[matlab camera calibration](https://www.mathworks.com/help/vision/ug/camera-calibration.html)

  extrinsic : world points -> camera coordinates : extrinsics parameters
  intrinsic : camera coordinates -> image plane : intrinsics parameters
  ![image](https://www.mathworks.com/help/vision/ug/calibration_cameramodel_coords.png)

[matlab one camera calibration](https://www.mathworks.com/help/vision/ug/single-camera-calibrator-app.html)

  reason : re-calculate the extrinsics, intrinsic
  How to evaluate : 
    re-projector error: +/- ??? (in pixel), lower = better
    examining camera extrinsics : see matlab page for detail
    View Undistorted Image      : see matlab page for detail


[opencv camera calibration and 3d reconstruction](https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html)

[Camera calibration With OpenCV cpp](https://docs.opencv.org/2.4/doc/tutorials/calib3d/camera_calibration/camera_calibration.html)

[camera calibration with opencv python](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_calib3d/py_calibration/py_calibration.html)
