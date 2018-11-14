### Elastic Stack Overview

The Elastic stack is a versatile collection of open source software tools that make gathering insights from data easier. 
Elastic Stack components:

1. Elasticsearch (Inverted& analyze storage built on top on Lucene)
2. Kibana (Web UI)
3. Logstash/Filebeat(logs and data aggregator. Logstash it's a lightweight aggregator who collects data and sends it to elasticsearh)
4. X-Pack (an elastic stack extension used for security, monitoring, alerting, reporting, graph, and machine learning)


![Elastic Stack 1](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/stack1.jpg)

![Elastic stack 2](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/stack2.png)

### Elasticsearch

Elasticsearch is a distributed, RESTful search and analytics engine and it is the heart of Elastic Stack. Elasticsearch is a document store. Elasticsearch is capable of storing documents as indexes in a similar fashion to .json format. Documents consists of "key":"value" pair

Ref: https://en.wikipedia.org/wiki/Elasticsearch

Elasticsearch is a search engine based on Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and released as open source under the terms of the Apache License.

### Elasticsearch vs SQL DBs

An Elasticsearch cluster can contain multiple indices (databases), which in turn contain multiple types (tables). These types hold multiple documents (rows), and each document has multiple fields (columns).

Elasticsearch structure differs from a traditional SQL database. Below you can find an analogy:

![Elastic vs. SQL](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/dbs_nosql_comparison.png)

Elastic stores documents in an inverted index. An inverted index holds the following information.
* the occurrence of a word in a document
* the position of that particular word in a document

![Inverted Index](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/inverted_index.png)

### Instalation

Will use Virtualbox to deploy Elasticsearch, but we could use as well a docker image or a cloud solution like Amazon or Elastic Cloud. 

Host OS Version | Guest OS Version
----------------|-----------------
4.18.0-kali2-amd64 | Debian Linux 9 


Update and upgrade package information.

```
apt-get update && apt-get upgrade
```

Enable ssh & http access from Host to Guest.

```
VBoxManage modifyvm "Elasticsearch" --natpf1 "host2guest-ssh,tcp,,2222,,22"
VBoxManage modifyvm "Elasticsearch" --natpf1 "host2guest-http,tcp,,9200,,9200"
```

Elasticsearch is a Java application, so you'll need to install a recent version OpenJDK.

```
apt-get install default-jdk
```

Add elasticsearch key and install version 6

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" |  tee -a /etc/apt/sources.list.d/elastic-6.x.list
apt-get update &&  apt-get install elasticsearch
```

Edit elasticsearch config file so we can have host http access

 ```
 nano /etc/elasticsearch/elasticsearch.yml
 Change network.host to 0.0.0.0 and host.http: 9200
 ```

Setup elasticsearch and deamonize elasticsearch service:

```
/bin/systemctl daemon-reload
/bin/systemctl enable elasticsearch.service
/bin/systemctl start elasticsearch.service
```

Verify if elasticsearch service has started succesfully:

```
service elasticsearch status
● elasticsearch.service - Elasticsearch
   Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-11-14 05:39:19 EST; 2min 49s ago
     Docs: http://www.elastic.co
 Main PID: 428 (java)
    Tasks: 38 (limit: 4915)
   CGroup: /system.slice/elasticsearch.service
           ├─428 /usr/bin/java -Xms1g -Xmx1g -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:+AlwaysPreTouch -Xss1m -Djava.awt.headless=true -Dfile.encoding=U
           └─672 /usr/share/elasticsearch/modules/x-pack-ml/platform/linux-x86_64/bin/controller

Nov 14 05:39:19 elastic systemd[1]: Started Elasticsearch.
```


```
curl -XGET 127.0.0.1:9200
{
  "name" : "JAgUjQT",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "WK1Kl1ovTeuNDMq8jLDerA",
  "version" : {
    "number" : "6.4.3",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "fe40335",
    "build_date" : "2018-10-30T23:17:19.084789Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Check http access from Host

![Host http access]