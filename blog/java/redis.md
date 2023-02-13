# REDIS

## Redis là gì? Tại sao phải dùng Redis?

Redis là tên viết tắt của Remote Dictionary Server (Máy chủ từ điển từ xa), đây là một kho lưu trữ dữ liệu dưới dạng key-value, trên bộ nhớ, mã nguồn mở, nhanh chóng.

![img.png](blog/java/img/redis.png)

## Ưu điểm

- Hoạt động lưu trữ key-value trên RAM cao.
- Cho phép phục hồi dữ liệu khi gặp sự cố nhờ việc lưu trữ dữ liệu trên đĩa cứng .
- Có khả năng phản hồi rất nhanh chỉ với vài mili giây để xử lý hàng triệu request mỗi giây.
- Tính năng sao chép đồng bộ giữa 2 cơ sở dữ liệu với nhau (replication) và tính năng cluster.


## Redis có các kiểu dữ liệu nào?
- String: là một trong những kiểu dữ liệu linh hoạt nhất redis. String là cấu trúc dữ liệu nhị phân và có thể lưu trữ đa dạng loại dữ liệu như số thập phân, ảnh JPEG, chuỗi,… Redis có thể làm việc với string hoặc từng phần của nó, đồng thời thực hiện tăng hay giảm các giá trị của float, integer.
- List: là một danh sách của strings, chứa tập hợp các phần tử chuỗi và được sắp xếp theo thứ tự insert. Redis có thể dễ dàng thêm một phần tử vào cuối hoặc đầu list. Vì việc truy xuất cực nhanh nên list rất phù hợp với các bài toán cần thao tác với nhiều phần tử gần đầu và cuối. Tuy nhiên việc thực hiện truy xuất các phần tử ở giữa list lại diễn ra rất chậm.
- Set: là tập hợp các string (đều không được sắp xếp). Redis có khả năng hỗ trợ các thao tác như đọc, thêm, xóa từng phần tử hay truy xuất, kiểm tra một phần tử xuất hiện trong tập hợp. Bên cạnh đó, redis còn hỗ trợ các phép tập hợp như lấy phần hợp, phần giao hay lấy phần khác nhau,…
- Hash: Việc lưu trữ hash table của các cặp key-value và trong đó các key được sắp xếp ngẫu nhiên, không theo bất kỳ thứ tự nào cả. Redis hỗ trợ các thao tác người dùng như đọc, thêm , xóa từng phần tử hoặc toàn bộ hash.
- Sorted set: là một danh sách được sắp xếp theo score, trong đó mỗi phần tử như là map của 1 string (member) và 1 floating-point number (score). Tương tự với set, redis cũng có thể thêm, xóa, đọc từng phần tử. Các phần tử của sorted set đều được sắp xếp theo thứ tự từ score nhỏ đến lớn.


## Cài đặt

```
pom.xml

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>s
    </dependency>
```
### Config application.yml
```text
application.yml

spring:
  cache:
    type: redis
  redis:
    host: localhost
    port: 6379
#// lecture by application
#    Password:
#    Timeout: 1000ms
#    lettuce:
#      pool:
#        max-active: 16
#        max-idle: 16
#        min-idle: 8
#        max-wait: 1ms
#        time-between-eviction-runs: 9000
```
### redis config để set dạng data sẽ được lưu trên redis:

### - STRING: new StringRedisSerializer() Ex: ``[{"roles":"admin"},{"roles":"guest"}]``
### - JSON: new GenericJackson2JsonRedisSerializer() Ex: ``"{\n  \"AccountID\": \"116C112250\",\n  \"Transaction_id\": \"8000301222000294\",\n  \"Transaction_time\": \"30-12-2022 16:21:54\",\n}"`` - các thông tin sẽ có thêm ký tự \

```
@Configuration
@EnableRedisRepositories
public class RedisConfig {

// way 1: use jedis rất thông dụng và sử dụng cho single thread
    @Bean
    JedisConnectionFactory jedisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplateJson() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new GenericJackson2JsonRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }

    @Bean
    public RedisTemplate<String, String> redisTemplateString(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, String> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        template.setDefaultSerializer(new StringRedisSerializer());
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new StringRedisSerializer());
        return template;
    }

// way 2: use lettuce. Letture hỗ trợ multithread nên sẽ rất an toàn nhưng phải dựa trên thực tế để đưa ra lựa chọn
//    @Bean
//    public RedisConnectionFactory redisConnectionFactory() {
//    return new LettuceConnectionFactory();
//}
//    @Bean
//    @Primary
//    public RedisTemplate<String, Shopping> redisTemplate(){
//        RedisTemplate<String, Shopping> empTemplate = new RedisTemplate<>();
//        empTemplate.setConnectionFactory(redisConnectionFactory());
//        return empTemplate;
//    }
}
```


### Redis hỗ trợ các định dạng

    redisTemplate.opsForValue().set(key)
    redisTemplate.opsForValue().get(key)

    redisTemplate.opsForList().put(key, data)
    redisTemplate.opsForList().get(key)

    redisTemplate.opsForHash().put(key, hashKey)
    redisTemplate.opsForHash().get(key, hashKey)

    redisTemplate.opsForSet().set(key, data)
    redisTemplate.opsForSet().get(key)