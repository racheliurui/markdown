title: AWS - MachineLearning
date: 2019-08-14 11:37:23
tags:
- AWS
- Machine Learning
---


# Hands-on

https://s3.amazonaws.com/solutions-reference/predictive-maintenance-using-machine-learning/latest/predictive-maintenance-using-machine-learning.pdf

```log
Couldn't call 'describe_notebook_instance' to get the Role ARN of the instance PredictiveMaintenanceNotebookInstance.
```
Update the role attached to the sagemaker instance

```
ResourceLimitExceeded
```

Change to train_instance_type = 'ml.p2.xlarge'


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
