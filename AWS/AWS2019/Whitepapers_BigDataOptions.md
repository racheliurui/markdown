title: AWS - CloudFormation 2019
date: 2019-08-01 11:37:23
tags:
- AWS White Paper
- Big Data

---

>https://d1.awsstatic.com/whitepapers/Big_Data_Analytics_Options_on_AWS.pdf

# Amazon Kinesis

- Amazon Kinesis Data Streams enables you to build custom applications that process or analyze streaming data.
  - Capture and store __terabytes__ of data per hour from __hundreds of thousands of sources__
  - Store a cursor in DynamoDB
- Amazon Kinesis Video Streams enables you to build custom applications that process or analyze streaming video.
- Amazon Kinesis Data Firehose enables you to deliver real-time streaming data to AWS destinations such as Amazon S3, Amazon Redshift, Amazon Kinesis Analytics, and Amazon Elasticsearch Service.
- Amazon Kinesis Data Analytics enables you to process and analyze streaming data with standard SQL.

# Lambda

- Default limit for concurrency is 1000

## Anti-pattern

- Long running
- Dynamic Websites
- Stateful Applications

# EMR

## Anti-pattern

- Small data set, Amazon EMR is built for massive parallel processing;
- ACID transaction requirements


# Glue


## Anti-Pattern

- Data Stearming
- Glue is PySpark based
- NoSQL DB not supported
