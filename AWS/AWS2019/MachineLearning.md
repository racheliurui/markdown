title: AWS - MachineLearning
date: 2019-08-14 11:37:23
tags:
- AWS
- Machine Learning
---

# Reference

>
https://youtu.be/GW0Bktm55nI


## AWS Machine Learning Stack

* ML Frameworks & Infrastructures
   * Frameworks
      * Tensorflow ( 85% TensorFlow workloads in cloud runs on AWS)
      * Apache Mxnet -- Deep learning for Enterprise dev ; liner scalable
      * Pytorch -- Facebook ; flexible , versatile and portable
      * AWS is framework agnostic
* ML Services
   *  SageMaker workflows
   *  SageMaker Ground Truth
   *  Use SageMaker to do Re-enforced ML : For example Vehicle routing
   *  Sagemaker Neo (Opensource)
        *  Accelerate the cycle of doing Machine learning
        *  CICD
        *  Optimize between different frameworks
* AI Services
    * Textract




## GE Healthcare Demo

* Neural Network Compression : reduce layer and retrain the model
* How to archive network compression using AWS service
   *  Use SageMaker RL
       * State current network archi
       * Action : remove layer or not
       * Reward : Accuracy + compression ratio
   * Result : 40% smaller model and 1%-2% loss of accuracy
