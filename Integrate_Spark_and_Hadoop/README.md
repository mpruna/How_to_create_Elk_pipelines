### Apache spark instalation

Download Spark and unzip

```
wget http://mirror.evowise.com/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
tar -xvf spark-2.4.3-bin-hadoop2.7.tgz
```
We need to specify which maven repo we have to use with our Elasticsearch version. Checkout the latest maven repo

```
root@elkstack:/home# curl http://127.0.0.1:9200
{
  "name" : "TIx499l",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "dTYAdtgvRHyZXBBJ04dBBw",
  "version" : {
    "number" : "6.8.2",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "b506955",
    "build_date" : "2019-07-24T15:24:41.545295Z",
    "build_snapshot" : false,
    "lucene_version" : "7.7.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Start Apache sparK with appropriate mvn **Scala/Elasticsearch** dependency

```
root@elkstack:/home/spark-2.4.3-bin-hadoop2.7# bin/spark-shell --packages org.elasticsearch:elasticsearch-spark-20_2.11:6.8.2
Ivy Default Cache set to: /root/.ivy2/cache
The jars for the packages stored in: /root/.ivy2/jars
:: loading settings :: url = jar:file:/home/spark-2.4.3-bin-hadoop2.7/jars/ivy-2.4.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
org.elasticsearch#elasticsearch-spark-20_2.11 added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent-aff5d863-711a-4bea-aa3a-8c54fbc75412;1.0
	confs: [default]
	found org.elasticsearch#elasticsearch-spark-20_2.11;6.8.2 in central
downloading https://repo1.maven.org/maven2/org/elasticsearch/elasticsearch-spark-20_2.11/6.8.2/elasticsearch-spark-20_2.11-6.8.2.jar ...
	[SUCCESSFUL ] org.elasticsearch#elasticsearch-spark-20_2.11;6.8.2!elasticsearch-spark-20_2.11.jar (748ms)
:: resolution report :: resolve 1572ms :: artifacts dl 759ms
```

Refs:
* https://spark.apache.org/downloads.html
* https://mvnrepository.com/



