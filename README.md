# Note
kafka note: https://www.learningcrux.com/course/apache-kafka-series-learn-apache-kafka-for-beginners-v2

# introduction to Kafka

Kafka is only use as transportation mechanism, for distributing messages.

01_target_&_Source.PNG

# Topics, partitions and offsets

## Topics:
* a paritcular steam of data
* similar to a table in a database. (without all the constraints)
* You can have as many topics as you want.
* A topic is identified by its name.

## Partitions
* topics are split into partitioins
* each partition is ordered, and begins with 0th position.

## offsets
* Each message within a partitions gets an icremental id, call offset.

02_topics_partitions_offset.PNG
03_topics_partitions_offset_example.PNG
04_topics_partitions_offset_details.PNG

# Brokers
Each broker has some kind of data, but not all the data, because Kafka is distributed.
05_brokers.PNG

## Brokers and topics

* In partition number and broker number, there is no relationship.
* When we create a topic, kafka will assign the topic and distributed it all the brokers.

06_brokers_and_topics.PNG

## Topic replication factor

Topic should have a replication factor - between 2, and 3 
(2 is bit risky , 3 is the standard)

* This replication factor is 2(replicating a top in 1 places) -> 07_Topic_replication_factor_2.PNG
* This replication factor is 2(replicating a top in 0 places) -> 06_brokers_and_topics.PNG

## Failure in broker
08_Failure_in_broker.PNG

## Leader for a partition
09_leader_partition.PNG

According to the figure given borker 101's partition is the leader partition, and boker 102's partition is replicating
things.
But if the broker 101 down, boker 102's partition 0 will be the leader partion.

So same thing happens in partition 1 as well.

# Procedure

When we send a data without a key, then the key sends round-robin Borker-1, Borker-2, Borker-3

`10_producer.PNG`, `11_producer_data_loss.PNG` , `11_producer_message_keys.PNG`

# Consumer
`12_consumer`, `12_consumer_group.PNG`, `12_consumer_group_inactive.PNG`

* one partition may have only one consumer at a time. If not that consumer will be inactive.

## consumer offset
`15_consumerOffsetPNG`

## dilivery semantics(අර්ථ විචාරය සම්බන්ධ) for consumers
`16_dilivery_symantics`
1. at most once -> එක් වරක්
2. at least once -> අවම වශයෙන් එක් වරක්
3. Exactly once -> හරියටම වරක්

# Broker Discovery
`17_boker_discovery.PNG`

# Zookeeper
`18_zookeeper.PNG`

## zookeeper cluster
`19_zookeper_cluster.PNG`

* zookeeper servers are connect to zookeeper.
* brokers are connected to zookeeper
* rights go to leader, followers has read
* kafka cluster is going to connect to zookeeper cluster.
* Automatically it'll understand.. one cluster is down, topics are created .. etc.
* So zookeeper is very important, kafka is correctly and well setup. but we don't dealing directly with zookeeper.
We just dealing with kafka brokers.

# Guarantees
`20_guarantees`

# overview
`21_overview.PNG`
* kafka cluster has brokers, inside brokers has topics.
* producer produce data to kafka cluster
* producer:
    - round robin: when there is a no key, so it send to all brokers.
    - key base ordering: If we sends data with a key, then the same key go to same partition
    - acks stategy: 0, 1 and or
    
# Install
1. install java 8
2. get apache kafka binary file.
    - extract the file and copy it to root (c:)
3. check
    - `java -version`
    - C:\kafka_2.12-2.4.1\bin\windows>`kafka-topics.bat`
```properties
    C:\kafka_2.12-2.4.1\bin\windows>kafka-topics.bat
    Create, delete, describe, or change a topic.
    Option                                   Description
    ------                                   -----------
    --alter                                  Alter the number of partitions,
                                               replica assignment, and/or
                                               configuration for the topic.
    --at-min-isr-partitions                  if set when describing topics, only
                                               show partitions whose isr count is
                                               equal to the configured minimum. Not
                                               supported with the --zookeeper
                                               option.
    --bootstrap-server <String: server to    REQUIRED: The Kafka server to connect
      connect to>                              to. In case of providing this, a
                                               direct Zookeeper connection won't be
                                               required.
```
This output will be there.

4. add path veriable
    -`C:\kafka_2.12-2.4.1\bin\windows`
    - check -> go to any directory and check -> `C:\Users\Ruchira.Supipi>kafka-topics.bat`
    
# Start zookeeper
1. create folder call data `C:\kafka_2.12-2.4.1\data`
2. in data create folder call `zookeeper`, `kafka`
3. configure `zookeeper.properties`
    - `C:\kafka_2.12-2.4.1\config` -> `zookeeper.properties` open in notepad++
    - paste zookeeper path: `C:\kafka_2.12-2.4.1\data\zookeeper`
    - make it as forward slashes.
```properties
dataDir=C:/kafka_2.12-2.4.1/data/zookeeper
```
4. start zookeeper server
`C:\kafka_2.12-2.4.1>zookeeper-server-start.bat config\zookeeper.properties`

5. configure `server.properties`
- `C:\kafka_2.12-2.4.1\config` -> `server.properties`
- add kafka folder path as bellow
```properties
log.dirs=C:/kafka_2.12-2.4.1/data/kafka
```
6. start kafka
`C:\kafka_2.12-2.4.1>kafka-server-start.bat config\server.properties`

# Kafka Topics CLI
`23_kakfa_CLI.PNG`
`C:\Users\Ruchira.Supipi>kafka-topics`

## create topic
`kafka-topics`
1. reference to zookeeper -> localhost:2181
`--zookeeper 127.0.0.1:2181`

2. give topic name
`--topic pipi_topic1` -> ``

3. create partitions
`--partitions 3`

4. give the replication factor
`--replication-factor 1`

5. create broker
    - brokers should be equals or greater than, replication factor
    `kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic1 --create --partitions 3 --replication-factor 2`
    
    - error:
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic2 --create --partitions 3 --replication-factor 2
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Error while executing topic command : Replication factor: 2 larger than available brokers: 1.
[2020-03-22 19:54:08,585] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 2 larger than available brokers: 1.
```
### 1. create topic
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic1 --create --partitions 3 --replication-factor 1

WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic pipi_topic1.
```
### 2. view List
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --list
__consumer_offsets
pipi_topic1

```

### 3. describe
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic  pipi_topic1 first_topic --describe
Topic: pipi_topic1      PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: pipi_topic1      Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic1      Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic1      Partition: 2    Leader: 0       Replicas: 0     Isr: 0
```
* leader 0 mean the broker: 0

`24_broker_id_0`

* delete topic `25_delete_topic`

#console producer CLI
## console
### documentation:
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer
```

### console producer
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1
>hello pipi
>I'm working
>learing kafka
>Terminate batch job (Y/N)? Y
```

### set properties
#### acks properties
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1 --producer-property acks=all
>some message that acked
>just for funTerminate batch job (Y/N)? Y
```

### When topic is not available

```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic2
>hey this topic does not exist!
[2020-03-22 23:19:02,926] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 3 : {pipi_topic2=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
[2020-03-22 23:19:03,041] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 4 : {pipi_topic2=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1 --producer-property acks=all
>now I have new topic
>Terminate batch job (Y/N)? Y
```
* here we produce a message, but this topic is not available, So it give worning for the first time.
* Then we enter the second message it doesn't give any error
* Because , goto kafka console and see. it has created a new topic there. `26_new_topic_kafka_log .PNG`

```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --list
__consumer_offsets
pipi_topic1
pipi_topic2
```
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic2 --describe
Topic: pipi_topic2      PartitionCount: 1       ReplicationFactor: 1    Configs:
        Topic: pipi_topic2      Partition: 0    Leader: 0       Replicas: 0     Isr: 0
```
This is created as default,

* So recomendation is always create the topic, Because the default is not good.

### change default settings

```properties
cmd> nano config/server.properties
```
- change value `num.partitions=3`
- exit and save
- restart kafka (press ctrl+ c -> `kafka-server-start.bat config\server.properties`)
- create new topic (default creating way)
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic3 --describe
Error while executing topic command : Topic 'pipi_topic3' does not exist as expected
[2020-03-22 23:44:18,972] ERROR java.lang.IllegalArgumentException: Topic 'pipi_topic3' does not exist as expected
        at kafka.admin.TopicCommand$.kafka$admin$TopicCommand$$ensureTopicExists(TopicCommand.scala:484)
        at kafka.admin.TopicCommand$ZookeeperTopicService.describeTopic(TopicCommand.scala:390)
        at kafka.admin.TopicCommand$.main(TopicCommand.scala:67)
        at kafka.admin.TopicCommand.main(TopicCommand.scala)
 (kafka.admin.TopicCommand$)

C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic3
>
[2020-03-22 23:45:04,372] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 3 : {pipi_topic3=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>
>hi
>no error now
>Terminate batch job (Y/N)? Y
```

- Then check, it has 3 partition as we configured as default.
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic3 --describe
Topic: pipi_topic3      PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: pipi_topic3      Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic3      Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic3      Partition: 2    Leader: 0       Replicas: 0     Isr: 0
```

# console consumer CLI
- description
```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer
```
## consume data
```properties

C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1

```
- though we entered lot of data it doesn't read all the topic. It read from the point it launch.
`27_consumer_producer.PNG`

## consume data from begining

*producer:*

```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1
>hi
>
>how are you
>pipi ary you getting well?
Processed a total of 10 messages
Terminate batch job (Y/N)? Y
```

*consumer*
```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1
hi

how are you
pipi ary you getting well?

```

## Consume messages from begining
```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1 --from-beginning
pipi ary you getting well?
how are you

C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1
>hi
>
>how are you
>pipi ary you getting well?
hi
```

`28_consumer-note.PNG` consumer note. 

## consumer group

### create consumer group
`29_consumer group.PNG`
```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1 --group my-app1
pipi1
```

* balancing load:

This happence, each consumer read from different pratitions.
consumers automatically balance the load.`30_consumer group_2.PNG`

* when one is shut-down one consumer:
`31_consumer group_shut_one.PNG`

* offset:

