### Logstash

Logstash is the server component that processes incoming logs and feeds to ES.
It's an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to your favorite “stash.” (Ours is Elasticsearch, naturally.)

['Official docs'](https://www.elastic.co/products/logstash)

!['Logstash'](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/logstash1.png)

Logstash capabilities:

    logstash parses, transforms, and filters data as it passes through.
    it can derive structure from unstructured data
    it can anonymize personal data or exclude it entirely
    it can do geo-location lookups
    it can scale across many nodes
    it guarantees at-least-once delivery
    it absorbs throughput from load spikes
    
### Download sample apache logs

```
wget http://media.sundog-soft.com/es/access_log
```

### Create logstash configuration file

['Logstash config ref'](https://www.elastic.co/guide/en/logstash/current/config-examples.html)

```
input {
    file{
        path => "/home/praslea/access_log"   # Specify log location
        type => "apache"                     # Specify log type i.e apache
        start_position => "beginning"        # Read older logs first
        sincedb_path => "/dev/null"
    }
}
filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
        match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]  #Setup date format
    }
}
output {
    elasticsearch {                          #
        hosts => ["localhost:9200"]
        document_type => "doc"
        }

    stdout {
        codec => rubydebug                    # Use rubydebug codec
    }
}
```

### Start logstash with the custom config file

```
 /usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf
```

After we startup, logstash will import logs from the file path specified in the config.

EX

```
[INFO ] 2018-11-15 04:38:23.513 [[main]<file] observingtail - START, creating Discoverer, Watch with file and sincedb collections
[INFO ] 2018-11-15 04:38:24.709 [Api Webserver] agent - Successfully started Logstash API endpoint {:port=>9601}
{
          "bytes" => "5639",
           "auth" => "-",
      "timestamp" => "30/Apr/2017:04:28:11 +0000",
           "verb" => "GET",
          "ident" => "-",
     "@timestamp" => 2017-04-30T04:28:11.000Z,
           "type" => "apache",
       "response" => "200",
        "message" => "216.244.66.246 - - [30/Apr/2017:04:28:11 +0000] \"GET /docs/triton/pages.html HTTP/1.1\" 200 5639 \"-\" \"Mozilla/5.0 (compatible; DotBot/1.1; http://www.opensiteexplorer.org/dotbot, help@moz.com)\"",
           "host" => "elastic",
       "referrer" => "\"-\"",
          "agent" => "\"Mozilla/5.0 (compatible; DotBot/1.1; http://www.opensiteexplorer.org/dotbot, help@moz.com)\"",
    "httpversion" => "1.1",
        "request" => "/docs/triton/pages.html",
           "path" => "/home/praslea/access_log",
       "clientip" => "216.244.66.246",
       "@version" => "1"
}
```

### Verify apache indices where created:

```
curl -XGET 127.0.0.1:9200/_cat/indices?v
health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-2017.05.05 emp7D1DIQkOoRi9SFh19tg   5   1      37137            0     14.6mb         14.6mb
yellow open   logstash-2017.05.03 FGkZBtwiQ2-KJMNf1L1XOA   5   1      42344            0       19mb           19mb
yellow open   logstash-2017.04.30 wfgmrxYRSRmENAeVVg-LYg   5   1      28582            0     11.4mb         11.4mb
yellow open   logstash-2017.05.01 2SjGyD55TxK_CCMcm3hRog   5   1      31896            0     14.2mb         14.2mb
yellow open   logstash-2017.05.04 GpRNuXAtROatLO66dqZDVw   5   1      33524            0     14.9mb         14.9mb
yellow open   logstash-2017.05.02 3JQOs5lnQKuZTFB3KA6eiw   5   1      32556            0     13.3mb         13.3mb
```