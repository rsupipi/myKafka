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
## write the code
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
## run project

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