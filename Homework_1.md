Big data problem:
E-commerce real-time transaction monitoring system


Yuchuan Xu   1801212958  
Fintech2018


1.	System introduction

"E-commerce real-time transaction monitoring system" is mainly used to help e-commerce platforms to monitor customers' purchase movements and order situation, and then use the results of data analysis to improve platform merchant efficiency. For example, merchants can design marketing activities based on real-time transaction conditions, or adjust inventory based on sales over time. On some major e-commerce event dates, such as "Double 11" and "Double 12", this kind of data platform is more useful. This system has high requirements for data timeliness.
The general data processing process of "E-commerce real-time is as follows: 
(a)	real-time capture of changes in transaction data in the database;
(b)	real-time calculation of indicators of each dimension of the order;
(c)	real-time passing indicators to the browser screen;
(d)	The above three stages can help to ensure the timeliness of the entire monitoring system, the delay can be controlled within seconds or sub-seconds.


2.	Key factors in the system

(a)	Comprehensive monitoring indicators;
(b)	Abnormal alarm;
(c)	Visual chart analysis


3.	Work flow

There are four main steps in this workflow:
Indicators collection -> Indicators processing -> Indicators storage -> Indicators visualization

![workfolw](https://raw.githubusercontent.com/YuchuanXu-1801212958/Homework_1/master/wf.jpg)


The arrow points to the path from the original data to the final display.
Transaction data binlog is stored in mysql, including order data, user registration data or user purchase information. With Alibaba Canal open source components, real-time monitoring of data changes and captures are passed to kafka.
Kafka is a large message queue buffer. It is a cluster mode message buffer. It can store a large amount of buffered data. If our transaction volume is large, we will use kafka as a message buffer to form some original transaction data.
After buffering, it will enter the real-time computing framework spark streaming. Spark streaming will process these order data in Kafka. Starting from spark streaming, there are two different monitoring methods.


Method 1:

Spark streaming processes the data into the metric we want, does some aggregation and index processing, and the metric is returned to kafka.
After processing the indicators, a service of nodejs will be started. This service will process the kafka of the metric again, and then push the data to the browser through socket.io. Then you will see that the entire data is extracted from the database. A series of transmissions are pushed to the browser in real time, and the real-time processing path is clear. As long as a transaction occurs in mysql, the entire data stream will finally reach the browser through such a pipeline.


Method 2:

After spark streaming has processed the basic data, it will be passed to HBASE. According to whether there are new indicators in Hbase, there are new indicators that show changes in transmission in the past, and the browser refreshes from time to time.
