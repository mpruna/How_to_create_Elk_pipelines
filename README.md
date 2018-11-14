### Elastic Stack Overview

The Elastic stack is a versatile collection of open source software tools that make gathering insights from data easier. 
Elastic Stack components:

1. Elasticsearch (Inverted& analyze storage built on top on Lucene)
2. Kibana (Web UI)
3. Logstash/Filebeat(logs and data aggregator. Logstash it's a lightweight aggregator who collects data and sends it to elasticsearh)
4. X-Pack (an elastic stack extension used for security, monitoring, alerting, reporting, graph, and machine learning)


[Elastic Stack 1](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/stack1.jpg)

[Elastic stack 2](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/stack2.png)

### Elasticsearch

Elasticsearch is a distributed, RESTful search and analytics engine and it is the heart of Elastic Stack. Elasticsearch is a document store. Elasticsearch is capable of storing documents as indexes in a similar fashion to .json format. Documents consists of "key":"value" pair

Ref: https://en.wikipedia.org/wiki/Elasticsearch

Elasticsearch is a search engine based on Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and released as open source under the terms of the Apache License.

### Elasticsearch vs SQL DBs

An Elasticsearch cluster can contain multiple indices (databases), which in turn contain multiple types (tables). These types hold multiple documents (rows), and each document has multiple fields (columns).

Elasticsearch structure differs from a traditional SQL database. Below you can find an analogy:

[Elastic vs. SQL](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/dbs_nosql_comparison.png)

Elastic stores documents in an inverted index. An inverted index holds the following information.
* the occurrence of a word in a document
* the position of that particular word in a document

[Inverted Index](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/inverted_index.png)

### Instalation

Will use Virtualbox to deploy Elasticsearch, but we could use as well a docker image or a cloud solution like Amazon or Elastic Cloud. 

Host OS Version | Guest OS Version
----------------|-----------------
4.18.0-kali2-amd64 | Debian Linux 9 



