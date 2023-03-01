# Kafka

## kafka là gì? Tại sao phải dùng kafka?

Kafka là một nền tảng message publish/subscribe phân tán (distributed messaging system) mã nguồn mở được xây dựng nhằm mục đích xử lý dữ liệu streaming theo thời gian thực.

### Kafka thường được sử dụng cho 2 mục đích chính sau:

- Publish và subscribe các stream of record (luồng dữ liệu)
- Lưu trữ các stream of record theo thứ tự
- Hỗ trợ xử lý stream of record theo thời gian thực

## Ưu điểm
- Open-source
- High-throughput: Có khả năng xử lý một lượng lớn thông tin một cách liên tục, gần như không có thời gian chờ
- High-frequency: Có thể xử lý cùng lúc nhiều message và nhiều thể loại topic
- Scalability: Dễ dàng mở rộng khi có nhu cầu
- Tự động lưu trữ message, dễ dàng kiểm tra lại
- Community: Cộng đồng Kafka hỗ trợ phát triển mạnh mẽ vì là opensource (mã nguồn mở) nên hệ thống luôn được cập nhật và hoàn thiện từng ngày.
## Nhược điểm
- Chưa có bộ công cụ giám sát hoàn chỉnh: Có nhiều tool khác nhau nhưng mỗi tool chỉ đáp ứng một tính năng quản lý nhất định, chẳng hạn như: Kafka tool (offset manager) GUI tool - quản lý topic và consumer, Lense - hỗ trợ query message, Akhq - toolbox quản lý Kafka và view data bên trong Kafka
- Không chọn được topic theo wildcard: Người dùng sẽ cần phải sử dụng chính xác tên topic để xử lý message
- Giảm hiệu suất: Kích thước message tăng khiến cho consumer và producer phải compress và decompress message, từ đó làm bộ nhớ bị chậm đi, ảnh hưởng đến throughput và hiệu suất.
- Xử lý chậm: Đôi khi số lượng queues trong Kafka cluster tăng đột biến khiến Kafka xử lý chậm hơn.


=>  Một số lợi ích nổi bật của Kafka bao gồm:

- Khả năng mở rộng cao: Mô hình phân vùng nhật ký của Kafka cho phép dữ liệu có thể được phân phối trên nhiều máy chủ và mở rộng máy chủ khi có nhu cầu.
  Tốc độ nhanh chóng: Việc xử lý thông qua tách các luồng dữ liệu giúp cho tốc độ trở nên nhanh hơn.
  Khả năng chịu lỗi và độ bền: Do các gói dữ liệu được sao chép và phân phối trên nhiều máy chủ khác nhau, nên khi có sự cố thì dữ liệu sẽ ít gặp lỗi và an toàn hơn.

## Một số khái niệm cơ bản trong Kafka

1. Producer

Producer là những application produce data và gửi data tới Kafka Server. Data này sẽ là những message có định dạng, được gửi dưới dạng mảng byte tới Kafka server. Ví dụ như các bạn có một tập tin .txt chứa text bên trong, chúng ta có thể dùng Producer để đọc từng dòng trong tập tin này rồi gửi tới Kafka server.

2. Consumer

Kafka sử dụng consumer để subscribe vào topic, các consumer được định danh bằng các group name. Nhiều consumer có thể cùng đọc một topic. Sau khi nhận được data, Consumer có thể thêm code để xử lý data theo nhu cầu của mình.

3. Cluster

Kafka cluster là một set các server, mỗi một set này được gọi là 1 broker.

4. Broker

Broker là Kafka server, là cầu nối giữa Message Publisher và Message Consumer, giúp chúng có thể trao đổi message với nhau.

5. Topic

Dữ liệu truyền trong Kafka theo topic, khi cần truyền dữ liệu cho các ứng dụng khác nhau thì sẽ tạo ra các topic khác nhau.

6. Partitions

Kafka là một distributed messaging system và chúng ta có thể setup Kafka server với cluster. Trong trường hợp một topic nhận quá nhiều message tại cùng một thời điểm, chúng ta có thể chia topic này thành những partitions được share giữa các Kafka server với nhau trong một cluster được handle các message này.

Một partition sẽ small và independent với các partitions khác. Số lượng partition cho mỗi topic thì tuỳ theo nhu cầu của ứng dụng mà chúng ta có thể quyết định.

7. Consumer Group

Consumer group là một group các Consumer consume message từ Kafka server. Mỗi một Consumer Group sẽ share với nhau việc handle message.

8. ZOOKEEPER: được dùng để quản lý và bố trí các broker.


![img.png](blog/java/img/kafka1.png)

![img.png](blog/java/img/kafka2.png)


# Example about kafka

## Config: Có 2 cách config:

- config trong application.yml
- config trong file config vs @configuraion và @Bean (thực chất nếu có cả 2 thì file @Bean này sẽ ghi đè lên fiel application.yml)

```java
=============================== way 1 ===============================
spring:
  kafka:
    consumer:
      bootstrap-servers: localhost:9092
      group-id: group_id
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer // nội dung đc gửi lên server dạng string
    producer:
      bootstrap-servers: localhost:9092
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer // nội dung đc gửi lên server dạng json
      properties:
        topics:
          testTopic: test_topic_application_yml

```

```java
@Data
@Configuration
@ConfigurationProperties(prefix = "spring.kafka")
public class KafkaConfig {
    ProducerConfig producer;

    @Data
    public static class ProducerConfig {
        Properties properties;
    }

    @Data
    public static class Properties {
        Topics topics;
    }

    @Data
    public static class Topics {
        String testTopic;
    }
}

// get topic file name from application.yaml
kafkaConfig.getProducer().getProperties().getTopics().getTestTopic();
```


```java
=============================== way 2 ===============================
@Configuration
class KafkaConsumerConfig {

    @Value("localhost:9092")
    private String bootstrapServers;

    @Bean
    public Map<String, Object> consumerConfigs() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "group-id");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        props.put(ConsumerConfig.AUTO_COMMIT_INTERVAL_MS_CONFIG, "100");
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, "15000");
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        return props;
    }

    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        return new DefaultKafkaConsumerFactory<>(consumerConfigs());
    }
// consume message là string
    @Bean
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, String>> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory =
                new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }

// consume message là json

    @Bean
    public ConsumerFactory<String, User> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, brokers);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, ErrorHandlingDeserializer.class);
        props.put(ErrorHandlingDeserializer.VALUE_DESERIALIZER_CLASS, JsonDeserializer.class);

        props.put(JsonDeserializer.VALUE_DEFAULT_TYPE, User.class);
        
        return new DefaultKafkaConsumerFactory<>(props);
    }

    @Bean
    KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, User>>
    kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, User> factory =
                new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }


}

@Configuration
class KafkaProducerConfig {

    @Value("localhost:9092")
    private String bootstrapServers;

// data push to kafka is tring
    @Bean
    public ProducerFactory<String, String> producerStringFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(
                ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,
                bootstrapAddress);
        configProps.put(
                ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,
                StringSerializer.class);
        configProps.put(
                ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,
                StringSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaStringTemplate() {
        return new KafkaTemplate<>(producerStringFactory());
    }

// data in Json
@Bean
    public ProducerFactory<String, String> producerJsonFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(
                ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,
                bootstrapAddress);
        configProps.put(
                ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,
                StringSerializer.class);
        configProps.put(
                ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,
                JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaJsonTemplate() {
        return new KafkaTemplate<>(producerJsonFactory());
    }

```

```java
application.yml
groupid.consumer: UATgroupId

/* Consumer */

@KafkaListener(topics = Constant.KAFKA.STOCK_EVENT_TOPIC, groupId = "${groupid.consumer:copytrade-batch-copier}")
@KafkaListener(
            topicPartitions = {
                    @TopicPartition(
                            topic = "demo",
                            partitionOffsets = {
                                    @PartitionOffset(partition = "0", initialOffset = "1")
                            }),
                    @TopicPartition(
                            topic = "topic1",
                            partitionOffsets = {
                                    @PartitionOffset(partition = "1", initialOffset = "0")
                            })
            }, groupId = "${groupid.consumer:default_groupId_name}) // nếu có define groupid.consumer: UATgroupId thì sẽ lắng nghe. còn ko thì sẽ là default_groupId_name
    public void listen(String message) {
        System.out.println("Received Message in group - group-id data: " + message);
    }
    
    
/* producer */

 public void sendMessage(String topic, int partition, String message) {
//        ListenableFuture<SendResult<String, String >> future = kafkaTemplate.send(topic,  message);
        ListenableFuture<SendResult<String, String >> future = kafkaTemplate.send(topic, partition, "key",  message);
        try {
            log.info("check request with timeout 10s");
            future.get(10, TimeUnit.SECONDS);
            log.info("Send request is done: " + future.isDone());
        }
        catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

        // add callback
        future.addCallback(new ListenableFutureCallback<SendResult<String, String>>() {
            @Override
            public void onSuccess(SendResult<String, String> result) {
                log.info("call send successful!");
            }

            @SneakyThrows
            @Override
            public void onFailure(Throwable ex) {
                throw new Exception("fail");
            }
        });
    }
```



