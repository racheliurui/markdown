title: AWS - Security best practise
date: 2018-05-13 09:13:23
tags:
- AWS Best practise
- Security
---

# Shared Responsibility Model

## 不同的类型服务有不同的shared responsibility model

* Infrastructure Service
  * including EC2, EBS, VPC
* Container Service
  * including RDS, EMR, AWS Elastic Beanstalk
    * 跟Infrastructure service相比，aws会负责OS以及OS上部署的应用平台
* Abstracted Service
  * Serveless
    * 跟上一种相比，aws还负责服务端和网络的加密；用户基本只负责客户端

## 推荐使用“Trusted Advisor”

* 除非要求买专业服务，这个安全报告是免费的。
* 检查内容包含常见端口检查

# 设计AWS安全的步骤方法论

## 第一步： Define and categorize Assets on AWS

最佳时间，列表：

| Asset名字| Owner| Category | Dependency | Cost|
|---------| ---| ---| ---|---|
| 系统名称比如LDAP| 谁在使用| 重要性，基础业务应用还是用来支持基础业务应用的网络和软件| 用了哪些aws的服务 | 谁出钱|

## 第二步： Design ISMS (Information Security Management System)

* 定义Scope
* 确定policy ：目标；遵循哪些法规；如何衡量风险；如何审批安全计划
* 确定风险评估方法： 业务需求；信息安全需要；信息技术支持；法律要求和责任
* 识别，分析评估和处理Risk
* Apply安全控制framework
* ISMS计划通过管理层审批
* document以上所有


# security strategy for AWS IAM service
