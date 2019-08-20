

### Kafka installation

1) Install Zookeeper

```
apt-get install zookeeperd
```

2) Install Kafka

```
wget http://mirrors.hostingromania.ro/apache.org/kafka/2.2.1/kafka-2.2.1-src.tgz
tar -xzvf kafka-2.2.1-src.tgz
```

### Start Kafka server

```
bin/kafka-server-start.sh config/server.properties
```

### Issue starting kafka server

```
root@elkstack:/home/kafka-2.2.1-src# bin/kafka-server-start.sh config/server.properties 
Classpath is empty. Please build the project first e.g. by running './gradlew jar -PscalaVersion=2.12.8'
```


### Kafka default config options

```
/home/kafka-2.2.1-src/config# cat server.properties | grep -v "#" | grep -v "^$"
broker.id=0
num.network.threads=3
num.io.threads=8
socket.send.buffer.bytes=102400
socket.receive.buffer.bytes=102400
socket.request.max.bytes=104857600
log.dirs=/tmp/kafka-logs
num.partitions=1
num.recovery.threads.per.data.dir=1
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1
log.retention.hours=168
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
zookeeper.connect=localhost:2181
zookeeper.connection.timeout.ms=6000
group.initial.rebalance.delay.ms=0
```
