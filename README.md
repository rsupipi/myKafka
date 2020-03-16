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

Topic should have a replication factor
    - between 2, and 3 (2 is the ideal, 3 is bit risky)

This replication factor is 2(replicating a top in 1 places) -> 07_Topic_replication_factor_2.PNG
This replication factor is 2(replicating a top in 0 places) -> 06_brokers_and_topics.PNG

## Failure in broker
08_Failure_in_broker.PNG

## Leader for a partition
09_leader_partition.PNG

According to the figure given borker 101's partition is the leader partition, and boker 102's partition is replicating
things.
But if the broker 101 down, boker 102's partition 0 will be the leader partion.

So same thing happens in partition 1 as well.

# Procedure

https://www.learningcrux.com/video/apache-kafka-series-learn-apache-kafka-for-beginners-v2/1/4


