---
layout: post
title: Elasticsearch for efficient and scalable telemetry lookup
comment: true
---  
## Introduction

  ![enter image description here](https://miro.medium.com/max/1400/1*BmvPfSSm2G8C-khX1rhCGg.png)

Elasticsearch is a distributed document-oriented search engine, which is used to store data in the form of a document. In this context, the document is a form of structured JSON format. As you might know, JSON (JavaScript Object Notation) is a lightweight data-interchange format.

  

It uses a RESTful search and analytics engine capable of addressing a growing number of use cases. FYI, a web API that obeys the REST constraints is informally described as RESTful. RESTful web APIs are typically loosely based on HTTP methods to access resources via URL-encoded parameters and the use of JSON or XML to transmit data.

  
  

## Motivation: Why Elasticsearch?

  

Suppose you want to query telemetry data collected from the ml-based systems. There are multiple options available such as the traditional RDB. If you want a highly distributed and efficient search engine, Elasticsearch is a good choice. Let’s consider this scenario. You are building a movie recommendation system like Netflix. To train and evaluate machine learning models in the system, you need to collect and query telemetry data about the user's movie-rating history. One challenge in this setting is that there’s tons of telemetry data. Without a per-user data store, fast search is nearly impossible.

  

### Fast search made possible by elasticsearch

Elasticsearch supports this to achieve efficient search. In traditional RDB, the data was stored like a tuple-based table below. It searches the data using SQL language and it takes a O(N) time, since it should search the entire table exhaustively.

  

![](https://lh4.googleusercontent.com/ibhPZXRj-Lv4S_3tN-lsaLd-KEukOUCmzsjw6Iep4Knq73fVP5m9gtpK2bv26v54_6qnR9e_Iey0chdusN5BdXqA05LWkVUaa7_tzkFldHu-ijJYDRFmrLGEs_nwbIDdwSm5FqbF)

However, in Elasticsearch, data is searched by O(1), because the data is stored in the manner of a per-text(term) document. It can find queried data using a hash table like below table.

![](https://lh5.googleusercontent.com/ezvNLpwvQRQKt1r5TAf-7Ppat8FmSMkVWKI02W1O7hATpOnIfWNSVoPiDW-CkwkLuZGkh9CPLXe6AFP5KRtWKlDVI9fsdwtStdb4ILUNP8oqzAGPNTT7CU_7wnATkcyLZ4BzNcLx)
  
 

  


### Scalable search made possible by elasticsearch

  

In this setting, scalability is also important. Since the telemetry log data keeps growing as long as the system is running, a single machine may not be sufficient and susceptible to the single-point-of-failure. This is why we need scalable systems. Elasticsearch also supports this. Elasticsearch cluster consists of multiple replicas of index and these replicas distribute across multiple machines in the cluster. When the read request is given to the cluster, Elasticsearch performs reading by utilizing multiple replicas - this results in higher throughput when reading data.

  
  

## Strength and limitations of Elasticsearch
### Strength

![](https://lh3.googleusercontent.com/nT9DK5lYb9U7rzDTLzFB96FZm0EOzHjvT_yqG6mKnXFUlntuluTfItb89NYRG-0uY8BPgLl6KeVrLrs3MxS4_2IaUan6HaCc6mqn9_qeB6eonQaYk32Dn81RIic6pkKyj0z4E1-p)

  

Elasticsearch supports fast search. It is attributed to its special indexing method. Aside from traditional relational DB, it stores data through the per-text(term) table which maps the relation between text(term) and document.

  

It can be scalable across multiple nodes. So eventually, you can start with a single node or two or three nodes. If the workload grows, in that case, you scale across multiple nodes. So, the more instances can be added to a cluster whenever needed. It is horizontally scalable.

  
  

### Limitations

It performs well for use cases of moderate size, but in case of streaming of TB's data per day, it either chokes or loses the data.

It is a flexible and powerful data storage search engine, but it is a bit difficult to use. Especially in terms of enterprise search usage, it is not as simple as out of the box search.

  

#### [reference]

[https://www.javatpoint.com/advantages-and-disadvantages-of-elasticsearch#Disadvantages](https://www.javatpoint.com/advantages-and-disadvantages-of-elasticsearch#Disadvantages)
