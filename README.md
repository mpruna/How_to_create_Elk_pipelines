Architecture overview

The Elastic stack is a versatile collection of open source software tools that make gathering insights from data easier.
Elastic Stack architecture is comprised of the following components

    Elasticsearch (Inverted& analyze storage buit on top on Lucene)
    Kibana (Web UI)
    Logstash/Filebeat(logs and data agregator. Logstash it's a lightweight aggregator who collects data and sends it to elasticsearh. )
    X-Pack (an elastic stack extension used for security, monitoring, alerting, reporting, graph, and machine learning)
   
![Elastik Stack 1](/root/Downloads/Section 2(Elastic stack overview)/elastic_archi.jpeg "Components")

![Elastik Stack 2](/root/Downloads/Section 2(Elastic stack overview)/Elastic_stack2.png "Elastic Stack")


Elasticsearch

Elasticsearch is a distributed, RESTful search and analytics engine and it is the heart of Elastic Stack.
Elasticsearch is a document store.
Elasticsearch is capable of storing documents as indexes in a similar fashion to .json format. Documents consits of "key":"value" pair

Ref: https://en.wikipedia.org/wiki/Elasticsearch

Elasticsearch is a search engine based on Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and is released as open source under the terms of the Apache License



