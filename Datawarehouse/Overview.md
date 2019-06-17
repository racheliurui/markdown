title: Data Wharehousing, Business Intelligence, and Dimensional Modeling Primer
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

## Publishing Metaphor for DW/BI Managers

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
这句话里面蕴含了3个dimension， “product”，”market“和”time”

Dimensional model常常使用关系型数据库，但是和3NF（normal form）模型不同。

* 3NF的目的是去除redundency， 属于ER （entity relationship）模型； Dimensional模型也属于ER模型
* 3NF和Dimential model的关键不同是normalization的程度
* 3NF的normalization程度更高，我们一般叫normalized model
* 3NF的缺点是复杂以及查询性能不好
* dimensional model易于用户理解；查询性能好，易于根据业务需求变化而变化

## Star Schemas Versus OLAP Cubes

* Dimensional model用关系型数据库实现就是Star Schema
* Dimensional model用多维数据库实现就是OLAP data cube

## OLAP Deployment Considerations

* Star Schema是基础
* OLAP的性能优势在被新技术蚕食（例如内存数据库，columnar DB）
* OLAP的表设计常常绑定技术提供商，移植性比较差。
* OLAP的数据安全性比较好；可以做到限制用户只能看到summary
* OLAP的分析能力更强大
* OLAP对变化的dimension支持更好
* OLAP支持snapshot fact但是不支持accumulate
* OLAP对hirarchy等类型的数据查询支持比较好

## Fact Tables for Measurements

* Each row in a fact table corresponds to a measurement event. 不能拆。

>  a measurement event in the physical world has a one-to-one
relationship to a single row in the corresponding fact table is a bedrock principle
for dimensional modeling

* Facts are often described as continuously valued to help sort out what is a fact
versus a dimension attribute.
    * Additivity fact : 销售额
    * Semi-Additivity fact： 例如account balance
    * Non-Additivity fact：例如产品单价
* textual Fact： 通常没有， 如果有也尽量放到Dimensional里面去
* Empty item. Fact 里面一定要放发生的事件，没有发生不要尝试放0.
* Fact表通常非常sparse； Fact表通常占据90%的存储； Fact表通常row非常大，column比较少；Fact表通常可以通过size预估行数
* Fact表分三种：transaction, periodic snapshot, and accumulating snapshot.
* Fact表至少有两个外键， 用来引用dimension表的主键
* __referential integrity__ 保证Fact表的条目引用的每个外键都正确
* __composite key__ Fact表的主键通常由所有的外键组合而成.

## Dimension Tables for Descriptive Context

* Dimension table 用来定义measurable业务事件的textual context
* Dimension 表描述who, what,when,where, how, why
* Dimension表通常列非常多，通常50-100个很正常
* Dimension表通常row少，column多
* Dimension表只有一个主键
* Dimension的attribute是主要的查询，分组以及报告label的来源
* Dimension的attribute名字必须有业务含义
* 例如如果一个code有前两个字段代表一个含义，后面两个字段代表一个含义，设计的时候最好单独出来一个dimension而不是让客户查询的时候manipulate字符串
* 实际设计的时候，如何确定一个numeric value是fact还是dimensional -- 确定它们是不是需要参与计算； 看数字是连续还是离散的

## Facts and Dimensions Joined in a Star Schema

Benefit of Star Schema

* Easy to understand
* Simplicity brings in performance benefits
* Dimensional model are gracefuly extensible to accommodate change.
  * Fact won't change, but dimension values can.
  * By adding new rows to dimension table or alter current fact table to add new dimension FK will fulfilll the change requirement


  A sample of SQL for star schema

  ```sql
  SELECT
  store.district_name,
  product.brand,
  sum(sales_facts.sales_dollars) AS "Sales Dollars"
  FROM
  store,
  product,
  date,
  sales_facts
  WHERE
  date.month_name="January" AND
  date.year=2013 AND
  store.store_key = sales_facts.store_key AND
  product.product_key = sales_facts.product_key AND
  date.date_key = sales_facts.date_key
  GROUP BY
  store.district_name,
  product.brand
  ```

  Where clauses including filter then join between fact and dimention then group by  to estabsh the aggregation.


P54.

# Kimball's DW/BI Architecture

4 components: Operational Source Systems, ETL system, Data PRZ area, BI applications

## Operational Source Systems

* Focusing on : Performance and availability
* Maintain little historical data

## Extract, Transformation, and Load System

* Extraction: move the data into DW scope
* Transformation: enrich, de-dup , etc
* Load the data into dimensional model
   * including Surrogate key assignment

> Industry argument, should ETL landing area be normalized structure? No need.


## Presentation Area to Support Business Intelligence

* Baseline: data must be dimensional schema or OLAP cubes ; this has been accepted by industry
* presentation area must contain atomic data (vs summary data );
    * it's __unacceptable__ to put atomic data in to 3NF model and only put summary data into star schema (WRONG)
* Data area should be around process measurement; and across organizational dep boundaries.
* When the bus architecture is used as a framework, you can develop the enterprise data warehouse in an agile, decentralized, realistically scoped, iterative manner.

>Data in the queryable presentation area of the DW/BI system must be dimensional, atomic (complemented by performance-enhancing aggregates), business process-centric, and adhere to the enterprise data warehouse bus architecture.The data must not be structured according to individual departments’ interpretation of the data.

## Business Intelligence Applications

* Tableau (??)

P59

## Restaurant Metaphor for the Kimball Architecture

*  ETL : Backend kitchen

ETL should focusing on ,
__Quality__
__Consistency__
__Integrity__

ETL should avoid being involved by DW/BI patrons.


*  Data Presentation and BI: Front Dining Room

Focusing on : properly organized and utilized to deliver as needed to the presentation area’s food, decor, service, and cost.

# Alternative DW/BI Architectures

## Independent Data Mart Architecture

* Data after multiple ETL logic landed in multiple models designed for different front room.
* No centralized data governance
* Short term low cost; normally already applied star schema for each model

## Hub-and-Spoke Corporate Information Factory Inmon Architecture

* 3NF is re-enforced

## Hybrid Hub-and-Spoke and Kimball Architecture

* 2 Layers of ETL
* Source -> 3NF -> Kimball


P66

# Dimensional Modeling Myths

## Myth 1: Dimensional only for summary Data

Summary data should complement the granular details solely to provide improved performance for common queries, __but not replace the details.__
The amount of history in dimensional models must only be driven by business's requirement nor the performance purpose.


## Myth 2: Dimensional for Departmental

## Myth 3: Dimensional are not scalable

It's common for fact table to have billions of rows; some fact table containing 2 trillion rows have been seen.
Key difference between 3NF and Dimensional is Dimensional are easier to understand.

## Myth 4: Dimensional only for predictable usage

The model is center on measurement process not pre-defined reports or analyses.
"God is in the details"

## Myth 5: Dimensional Can't be integrated

Data integration depends on standardized labels, values, and definitions.

# More Reasons to Think Dimensionally

Robust dimensions translate into robust DW/BI systems.

# Agile Considerations

# Summary
