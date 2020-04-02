#  kafka java programming

## add maven dependancy
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

## start servers
* zookeeper: 
`C:\kafka_2.12-2.4.1> zookeeper-server-start.bat config\zookeeper.properties`

* Kafka: 
`C:\kafka_2.12-2.4.1> kafka-server-start.bat config\server.properties`
https://www.learningcrux.com/course/apache-kafka-series-learn-apache-kafka-for-beginners-v2

## 2 kafka documentation
https://kafka.apache.org/documentation/

## 