### Logstash

Logstash is the server component that processes incoming logs and feeds to ES.
It's an open source, server-side data processing pipeline that ingests data from a multitude of sources simultaneously, transforms it, and then sends it to your favorite “stash.” (Ours is Elasticsearch, naturally.)

['Logstash Ref'](https://www.elastic.co/products/logstash)

!['Logstash'](https://github.com/mpruna/IMPORTING_DATA_INTO_ELASTICSEARCH/blob/master/images/logstash1.png)

Logstash capabilities:

    logstash parses, transforms, and filters data as it passes through.
    it can derive structure from unstructured data
    it can anonymize personal data or exclude it entirely
    it can do geo-location lookups
    it can scale across many nodes
    it guarantees at-least-once delivery
    it absorbs throughput from load spikes
    
