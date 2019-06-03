title: Different Worlds of Data Capture and Data Analysis
date: 2019-06-02 20:28:10
tags:
- Datawarehouse
- BI
---

# Key difference between operational system and Data warehouse

* 一个往里面送数据，一个往外查数据
* 一个要求transaction并且保持当前状态准确，业务逻辑严格按照流程来；一个要求大量查询和比对，查询需求不停变化

# Goals of Data Warehousing and Business Intelligence

* 收集的数据不好用
* 收集的数据不是查询友好
* 业务人员用起来不方便
* 数据不一致
* 我们想实现fact-based决策

所以DW需要，
* 数据贴近业务人员；好理解
* consistent： 一样的名字必须代表一样的东西
* 能够支持需求变化，能够支持变化的时候对用户透明
* 数据必须及时，即使需要clean和validate
* 数据安全非常重要， DW的信息决定了一个企业“卖什么东西给谁以什么价格”
* DW是一个decision support system
* DW必须得到业务人员的支持和使用才能成功；跟业务系统不一样，DW是optional，不好用就会被废弃

# Publishing Metaphor for DW/BI Managers

把DW必须成发行杂志。DW需要

* 理解读者
* 取悦读者
* 保证发行

类似于发行杂志，DW需要选择数据源，保证数据准确，然后以正确的方式展现给读者（用户），定期更新。

# Dimensional Modeling Introduction

Dimensional modeling实现了两个难点：

* 易于被业务user理解
* 查询快速

“We sell products in various markets and measure our performance over time”
这句话里面蕴含了3个dimention， “product”，”market“和”time”

Dimentional model常常使用关系型数据库，但是和3NF（normal form）模型不同。

* 3NF的目的是去除redundency， 属于ER （entity relationship）模型； Dimentional模型也属于ER模型
* 3NF和Dimential model的关键不同是normalization的程度
* 3NF的normalization程度更高，我们一般叫normalized model
* 3NF的缺点是复杂以及查询性能不好
* dimentional model易于用户理解；查询性能好，易于根据业务需求变化而变化

# Star Schemas Versus OLAP Cubes

* Dimentional model用关系型数据库实现就是Star Schema
* Dimentional model用多维数据库实现就是OLAP data cube

## OLAP Deployment Considerations

* Star Schema是基础
* OLAP的性能优势在被新技术蚕食（例如内存数据库，columnar DB）
* OLAP的表设计常常绑定技术提供商，移植性比较差。
* OLAP的数据安全性比较好；可以做到限制用户只能看到summary
* OLAP的分析能力更强大
* OLAP对变化的dimention支持更好
* OLAP支持snapshot fact但是不支持accumulate
* OLAP对hirarchy等类型的数据查询支持比较好

# Fact Tables for Measurements

* Each row in a fact table corresponds to a measurement event. 不能拆。

>  a measurement event in the physical world has a one-to-one
relationship to a single row in the corresponding fact table is a bedrock principle
for dimensional modeling

* Facts are often described as continuously valued to help sort out what is a fact
versus a dimension attribute.
    * Additivity fact : 销售额
    * Semi-Additivity fact： 例如account balance
    * Non-Additivity fact：例如产品单价
* textual Fact： 通常没有， 如果有也尽量放到Dimentional里面去
* Empty item. Fact 里面一定要放发生的事件，没有发生不要尝试放0.
* Fact表通常非常sparse； Fact表通常占据90%的存储； Fact表通常row非常大，column比较少；Fact表通常可以通过size预估行数
* Fact表分三种：transaction, periodic snapshot, and accumulating snapshot.
* Fact表至少有两个外键， 用来引用dimention表的主键
* __referential integrity__ 保证Fact表的条目引用的每个外键都正确
* __composite key__ Fact表的主键通常由所有的外键组合而成.

## Dimension Tables for Descriptive Context

* Dimention table 用来定义measurable业务事件的textual context
* Dimention 表描述who, what,when,where, how, why
* Dimention表通常列非常多，通常50-100个很正常
* Dimention表通常row少，column多
* Dimention表只有一个主键
* Dimention的attribute是主要的查询，分组以及报告label的来源
* Dimention的attribute名字必须有业务含义
* 例如如果一个code有前两个字段代表一个含义，后面两个字段代表一个含义，设计的时候最好单独出来一个dimention而不是让客户查询的时候manipulate字符串
* 实际设计的时候，如何确定一个numeric value是fact还是dimentional -- 确定它们是不是需要参与计算； 看数字是连续还是离散的

P52
