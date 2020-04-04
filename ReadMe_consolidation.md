# Note

***tutorial:***
kafka note: https://www.learningcrux.com/course/apache-kafka-series-learn-apache-kafka-for-beginners-v2

***kafka documentation***
https://kafka.apache.org/documentation/

# 1. introduction to Kafka

Kafka is only use as transportation mechanism, for distributing messages.

01_target_&_Source.PNG

# 2. Topics, partitions and offsets

## 2.1 Topics:
* a paritcular steam of data
* similar to a table in a database. (without all the constraints)
* You can have as many topics as you want.
* A topic is identified by its name.

## 2.2 Partitions
* topics are split into partitioins
* each partition is ordered, and begins with 0th position.

## 2.3 offsets
* Each message within a partitions gets an icremental id, call offset.

02_topics_partitions_offset.PNG
03_topics_partitions_offset_example.PNG
04_topics_partitions_offset_details.PNG

# 2.4 Brokers
Each broker has some kind of data, but not all the data, because Kafka is distributed.
05_brokers.PNG

## 2.5 Brokers and topics

* In partition number and broker number, there is no relationship.
* When we create a topic, kafka will assign the topic and distributed it all the brokers.

06_brokers_and_topics.PNG

## 2.6 Topic replication factor

Topic should have a replication factor - between 2, and 3 
(2 is bit risky , 3 is the standard)

* This replication factor is 2(replicating a top in 1 places) -> 07_Topic_replication_factor_2.PNG
* This replication factor is 1(replicating a top in 0 places) -> 06_brokers_and_topics.PNG

## 2.7 Failure in broker
08_Failure_in_broker.PNG

## 2.8 Leader for a partition
09_leader_partition.PNG

According to the figure given borker 101's partition is the leader partition, and boker 102's partition is replicating
things.
But if the broker 101 down, boker 102's partition 0 will be the leader partion.

So same thing happens in partition 1 as well.

# 3. Procedure

When we send a data without a key, then the key sends round-robin Borker-1, Borker-2, Borker-3

`10_producer.PNG`, `11_producer_data_loss.PNG` , `11_producer_message_keys.PNG`

# 4. Consumer
`12_consumer`, `12_consumer_group.PNG`, `12_consumer_group_inactive.PNG`

* one partition may have only one consumer at a time. If not that consumer will be inactive.

## 4.1 consumer offset
`15_consumerOffsetPNG`

## 4.2 dilivery semantics(අර්ථ විචාරය සම්බන්ධ) for consumers
`16_dilivery_symantics`
1. at most once -> එක් වරක්
2. at least once -> අවම වශයෙන් එක් වරක්
3. Exactly once -> හරියටම වරක්

# 5. Broker Discovery
`17_boker_discovery.PNG`

# 6. Zookeeper
`18_zookeeper.PNG`

## 6.1 zookeeper cluster
`19_zookeper_cluster.PNG`

* zookeeper servers are connect to zookeeper.
* brokers are connected to zookeeper
* rights go to leader, followers has read
* kafka cluster is going to connect to zookeeper cluster.
* Automatically it'll understand.. one cluster is down, topics are created .. etc.
* So zookeeper is very important, kafka is correctly and well setup. but we don't dealing directly with zookeeper.
We just dealing with kafka brokers.

# 7. Guarantees
`20_guarantees`

# 8. overview
`21_overview.PNG`
* kafka cluster has brokers, inside brokers has topics.
* producer produce data to kafka cluster
* producer:
    - round robin: when there is a no key, so it send to all brokers.
    - key base ordering: If we sends data with a key, then the same key go to same partition
    - acks stategy: 0, 1 and or

# 9. Work with Kafka
## 9.1 Install
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
    
## 9.2 zookeeper configurations
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

## 9.3 kafka configutation
5. configure `server.properties`
- `C:\kafka_2.12-2.4.1\config` -> `server.properties`
- add kafka folder path as bellow
```properties
log.dirs=C:/kafka_2.12-2.4.1/data/kafka
```
6. start kafka
`C:\kafka_2.12-2.4.1>kafka-server-start.bat config\server.properties`

# 10. Start Kafka

**start servers:**
* zookeeper: 
`C:\kafka_2.12-2.4.1> zookeeper-server-start.bat config\zookeeper.properties`

* Kafka: 
`C:\kafka_2.12-2.4.1> kafka-server-start.bat config\server.properties`

## 1. Kafka Topics CLI

### 1.0 Documentation
`23_kakfa_CLI.PNG`
`C:\Users\Ruchira.Supipi>kafka-topics`

### 1.1 Create topic

`kafka-topics`
1. reference to zookeeper -> localhost:2181
`--zookeeper 127.0.0.1:2181`

2. give topic name
`--topic pipi_topic1` ->

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
- Here we have created one broker and the replication factor is 2

***create topic***
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic1 --create --partitions 3 --replication-factor 1

WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic pipi_topic1.
```
### 1.2 view List
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --list
__consumer_offsets
pipi_topic1

```

### 1.3 describe
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic  pipi_topic1 --describe
Topic: pipi_topic1      PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: pipi_topic1      Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic1      Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: pipi_topic1      Partition: 2    Leader: 0       Replicas: 0     Isr: 0
```
* leader 0 mean the broker: 0

`24_broker_id_0`

* delete topic `25_delete_topic`

## 2. Console producer CLI

### 2.0 documentation
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer
```

### 2.1 console producer
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1
>hello pipi
>I'm working
>learing kafka
>Terminate batch job (Y/N)? Y
```

### 2.2 set properties

***acks properties***
```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1 --producer-property acks=all
>some message that acked
>just for funTerminate batch job (Y/N)? Y
```

### 2.3 When topic is not available

```properties
C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic2
>hey this topic does not exist!
[2020-03-22 23:19:02,926] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 3 : {pipi_topic2=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
[2020-03-22 23:19:03,041] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 4 : {pipi_topic2=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)
>C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1 --producer-property acks=all
>now I have new topic
>Terminate batch job (Y/N)? Y
```
* here we produce a message, but this topic is not available, So it give warning for the first time.
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

### 2.4 change default settings

```properties
cmd> nano config/server.properties
```
- change value `num.partitions=3`
- exit and save
- restart kafka (press ctrl+ c -> `kafka-server-start.bat config\server.properties`)
- create new topic (default creating way)

*check availability of*
```properties
C:\Users\Ruchira.Supipi>kafka-topics --zookeeper 127.0.0.1:2181 --topic pipi_topic3 --describe
Error while executing topic command : Topic 'pipi_topic3' does not exist as expected
[2020-03-22 23:44:18,972] ERROR java.lang.IllegalArgumentException: Topic 'pipi_topic3' does not exist as expected
        at kafka.admin.TopicCommand$.kafka$admin$TopicCommand$$ensureTopicExists(TopicCommand.scala:484)
        at kafka.admin.TopicCommand$ZookeeperTopicService.describeTopic(TopicCommand.scala:390)
        at kafka.admin.TopicCommand$.main(TopicCommand.scala:67)
        at kafka.admin.TopicCommand.main(TopicCommand.scala)
 (kafka.admin.TopicCommand$)
```
* Create a new producer for a topic which is not exist.
```properties
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

## 3. console consumer CLI

### 3.0 description
```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer
```
### 3.1 consume data
```properties

C:\Users\Ruchira.Supipi>kafka-console-producer --broker-list 127.0.01:9092 --topic pipi_topic1

```
- though we entered lot of data it doesn't read all the topic. It read from the point it launch.
`27_consumer_producer.PNG`

## 3.2 real time produce and consume

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

* in kafka messages are consumed in real time.

## 3.3 Consume messages from begining

* If you want to get all data(from the beginning)

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

## 3.4 consumer group

### 3.4.1 create consumer group
* with 2 consumer: `29_consumer group.PNG`
* with 3 consumer: `30_consumer group_2.PNG`

```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1 --group my-app1
pipi1
```

### 3.4.2 balancing load:

This happen, when each consumer read from different pratitions.
consumers automatically balance the load.`30_consumer group_2.PNG`

* when one is shut-down one consumer: `31_consumer group_shut_one.PNG`

### 3.4.3 Keeping offsets when consuming data
- when we consume data for the fist time from the beginning it gives all data. and it keeps an offset.
- So for the second time we request data it gives data from the offset.
- Hence all data before the offset will not be there.
`32_keeping_offset.jpg` `33_keeping_offset.jpg.PNG`
```properties
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1 --group my-app2 --from-beginning
```
33_keeping_offset.jpg

### 3.4.4 Consumer group CLI

***Discription:***
```properties
C:\kafka_2.12-2.4.1>kafka-consumer-groups
```

***List***
```properties
C:\kafka_2.12-2.4.1>kafka-consumer-groups --bootstrap-server localhost:9092 --list
my-app1
group_string
my-app2
```
* If we doesn't specify a consumer, it generates a random console consumer.`34_console_consumer.png`

***Describe***
```properties
C:\kafka_2.12-2.4.1>kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-app1

Consumer group 'my-app1' has no active members.

GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
my-app1         pipi_topic1     2          8               8               0               -               -               -
my-app1         pipi_topic1     1          16              16              0               -               -               -
my-app1         pipi_topic1     0          9               9               0               -               -               -

```

* `Consumer group 'my-app1' has no active members.` Because we have stop all console consumers.

***LAG***

* LAG is `35_LAG.PNG`

***Active member:***    
`36_start_consumer_and_consumer_group`

## 4.Resetting Offsets

- get option list
```properties
C:\kafka_2.12-2.4.1>kafka-consumer-groups
```
```properties
--offsets                               Describe the group and list all topic
                                          partitions in the group along with
                                          their offset lag. This is the
                                          default sub-action of and may be
                                          used with '--describe' and '--
                                          bootstrap-server' options only.
                                        Example: --bootstrap-server localhost:
                                          9092 --describe --group group1 --
                                          offsets
--reset-offsets                         Reset offsets of consumer group.
                                          Supports one consumer group at the
                                          time, and instances should be
                                          inactive
                                        Has 2 execution options: --dry-run
                                          (the default) to plan which offsets
                                          to reset, and --execute to update
                                          the offsets. Additionally, the --
                                          export option is used to export the
                                          results to a CSV format.
                                        You must choose one of the following
                                          reset specifications: --to-datetime,
                                          --by-period, --to-earliest, --to-
                                          latest, --shift-by, --from-file, --
                                          to-current.
                                        To define the scope use --all-topics
                                          or --topic. One scope must be
                                          specified unless you use '--from-
                                          file'.
```

***Reset Offset***
```properties
C:\kafka_2.12-2.4.1>kafka-consumer-groups --bootstrap-server localhost:9092 --group my-app1 --reset-offsets --to-earliest --execute --topic pipi-topic1
```
`37_reset_offest.PNG`

* If we run console consumer again it'll display all messages `38_RunConsole_consumer.PNG`
* So now LAG is 0. `38_LAG_0.PNG`

***shift by***
* we can shift offset to frontward or backward.
    -eg: by adding (2) and (-2) `39_shift.PNG`
* then check it by using console consumer `40_consumer_after_shifting.PNG`

# 5. Kafka Tool UI
- Download kafka tool `http://www.kafkatool.com/download.html`
- configuration   `41_kafka_ui_test.PNG`
- view messages `42_view_messages.PNG`

# 6. kafka java programming

## 6.1 add maven dependancy
* project -> new -> maven project
* Add maven dependency, and comment    `<scope>test</scope>`
```xml
        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>2.4.1</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.30</version>
<!--            <scope>test</scope>-->
        </dependency>
```
## 6.2 Producer
### 6.2.1 Producer callback
```java
public class ProducerDemo {
    public static void main(String[] args) {

        final Logger logger = LoggerFactory.getLogger(ProducerDemo.class);
//        final String bootstrapServers = "127.0.0.1:9092";     // Same as local host
        final String bootstrapServers = "localhost:9092";


        /********* create Producer properties *********/
        Properties properties = new Properties();

//        properties.setProperty("bootstrap.servers", bootstrapServers);
//        properties.setProperty("key.serializer", StringSerializer.class.getName());
//        properties.setProperty("value.serializer", StringSerializer.class.getName());

        properties.setProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        properties.setProperty(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.setProperty(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());


        /********** create the producer **************/
        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);

        ProducerRecord<String, String> record =
                new ProducerRecord<String, String>("pipi_topic1","Hi pipi.. callback");

        /********** send data - asyncronous  **************/
        producer.send(record, new Callback() {
            public void onCompletion(RecordMetadata recordMetadata, Exception e) {
                // executes every time  a record is successfully sent or an exception is trown.
                if (e == null) {
                    // the record was successfully sent.
                    logger.info("Received new metadata. \n"+
                            "Topic: " + recordMetadata.topic() + "\n" +
                            "Partition: " + recordMetadata.partition() + "\n" +
                            "Offset: " + recordMetadata.offset() + "\n" +
                            "Timestamp: " + recordMetadata.timestamp() + "\n");
                }
            }
        });

        /********** flush data  **************/
        producer.flush();

        /********** flush  and close producer  **************/
        producer.close();

    }
```
run:

**a. start servers:**
* zookeeper: 
`C:\kafka_2.12-2.4.1> zookeeper-server-start.bat config\zookeeper.properties`

* Kafka: 
`C:\kafka_2.12-2.4.1> kafka-server-start.bat config\server.properties`
https://www.learningcrux.com/course/apache-kafka-series-learn-apache-kafka-for-beginners-v2

**b. run the main class**

**c. check it on consumer console**
 ```properties
C:\Users\Ruchira.Supipi>kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic pipi_topic1 --group my-app1
```
output:
43_java_callBack.PNG

