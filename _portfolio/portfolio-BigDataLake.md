---
title: "Big Data Lake Solution for Warehousing Stock Data and Tweet Data"
excerpt: "We want to build a large-scale data framework that will enable us to store and analyze financial market data and drive future predictions for inverstment.<br/><img src='/images/datawarehouse_stocks_twitter/project_overview.png'>"
collection: portfolio
---

Created by [Stuart Miller](https://github.com/sjmiller8182), 
[Paul Adams](https://github.com/PaulAdams4361), and 
Rikel Djoko

# Project Summary

The objective of this project was to build a large-scale data framework that will enable us to store and analyze financial market data and drive future predictions for inverstment.

For this project, the following types of data were used

* Daily stock prices for all companies traded on the NYSE and the NASDAQ.
* Intra-day values for all companies traded on the NYSE and the NASDAQ.
  * Prices: high, low, open, close,
  * Supporting Values: Brollinger Bands, stochastic oscillators, and moving average CD
  * Intra-day values are at 15 minute intervals
* Tweets from over [100 investment related twitter accounts](https://github.com/sjmiller8182/DBMS_Proj/blob/master/scrape_utils/python/twitter_handles.txt)

## Data Warehouse Overview

Two star schemas were designed for this data warehouse: a normalized schema (3NF) and a denormalized schema (1NF).
We investigated the performance of the two schemes in the context of this problem.
Conceptual diagrams of the data warehouse schemas are shown below.

**Conceptual Diagram of the Data Warehouse Snowflake Schema**

<br/><img src='/images/datawarehouse_stocks_twitter/SnowFlake_Schema_Simple.png' width="400" height="300"> 

**Conceptual Diagram of the Data Warehouse Denormalized Star Schema**

<br/><img src='/images/datawarehouse_stocks_twitter/Star_Schema_Simple.png' width="300" height="200">

## Big Data Solution Implementation

### Data Collection

Data collection was operated by an AWS elastic compute 2 (EC2) instance.
It queried Twitter data from the [Twitter API](https://developer.twitter.com/en/docs)
 and stock data from the [AlphaVantage API](https://www.alphavantage.co/).
As the data was collected, it was pushed into a long-term storage archive.
The data collection systems were implemented in R and python.

### Data Storage (Long-term)

For data storage, AWS S3 was used.
It is used to store and retrieve any amount of data any time, from anywhere on the web.
S3 is reliable, fast, and inexpensive. 
S3 was used to share and load data directly into HDFS.

### Data Lake Implementation

The data was trasferred into a Hadoop data lake for this project.
Elastic MapReduce (EMR), from AWS, was used for the implementation of
 [Hadoop](https://hadoop.apache.org/) in this project.
AWS EMR is a pre-configured compute cluster for Big Data.
The cluster can be provisioned and terminated on demand as needed.
It comes with a configurable set of the Hadoop ecosystem elements pre-installed and ready to use.
The EMR cluster used in this study was provisioned with three
 [m5.xlarge](https://aws.amazon.com/ec2/instance-types/m5/) elastic compute instances using version 5.27.0 of the
 [EMR software](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-release-5x.html).
[Apache Hive](https://hive.apache.org/) was used to create the data warehouse from the data lake.
[Cloudera Hue](https://gethue.com/) was used for interfacing with the Hadoop cluster.

<br/><img src='/images/datawarehouse_stocks_twitter/Big_Data_Solution_AWS.png'>

More information about Amazon Web Serves can be found at [https://aws.amazon.com/](https://aws.amazon.com/)

## Results


Queries were run on the two schemas with different EMR cluster sizes to see the impact of normalization on query time.
The collected data is located [here](https://github.com/sjmiller8182/DBMS_Proj/blob/master/results_analysis/results.csv).
A plot summarizing the results is shown below.

<br/><img src='/images/datawarehouse_stocks_twitter/Rplot.png' width="450" height="400"/>

## Additional Information


* The project repository can be found at [https://github.com/sjmiller8182/Warehousing-Stock-Tweet-Data](https://github.com/sjmiller8182/Warehousing-Stock-Tweet-Data)
* The project paper can be found [here](https://github.com/sjmiller8182/Warehousing-Stock-Tweet-Data/blob/master/reports/Project_Paper.pdf)


