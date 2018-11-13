### Elastic Stack Overview

The Elastic stack is a versatile collection of open source software tools that make gathering insights from data easier.
Elastic Stack architecture is comprised of the following components:
1. Elasticsearch (Inverted& analyze storage buit on top on Lucene)
2. Kibana (Web UI)
3. Logstash/Filebeat(logs and data agregator. Logstash it's a lightweight aggregator who collects data and sends it to elasticsearh)
4. X-Pack (an elastic stack extension used for security, monitoring, alerting, reporting, graph, and machine learning)
  

[Elastic Stack 1](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/stack1.jpg)

[Elastic stack 2](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/stack2.png)



### Elasticsearch


Elasticsearch is a distributed, RESTful search and analytics engine and it is the heart of Elastic Stack.
Elasticsearch is a document store.
Elasticsearch is capable of storing documents as indexes in a similar fashion to .json format. Documents consits of "key":"value" pair

Ref: https://en.wikipedia.org/wiki/Elasticsearch

Elasticsearch is a search engine based on Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and is released as open source under the terms of the Apache License


