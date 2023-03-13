# Object mapper

Object mapper là 1 cách để convert String/Json sang dạng object. Thông thường thì sẽ dùng để kết hợp đẩy lên redis và lấy thông tin từ redis về. Hoặc parse token

## Cài đặt
```java
pom.xml

    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.14.0-rc1</version>
    </dependency>

// convert map to json
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.14.0-rc1</version>
    </dependency>
```


```java
@Configuration
public class ObjectMapperConfig {
    @Bean
    ObjectMapper objectMapper() {
        return new ObjectMapper();
    }

    @Bean
    ObjectMapper objectMapperDisableUnknownProperties() {
        return new ObjectMapper().disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
    }
}
```

## Sử dụng

### Các method

- writeValueAsString
- readValue


```java

 private final ObjectMapper objectMapperDisableUnknownProperties;

String json = objectMapperDisableUnknownProperties.writeValueAsString(map);
return objectMapperDisableUnknownProperties.readValue(json, HeaderMapper.class);
```
### Object mapper cho token
```java
    public TokenMapper getToken() throws JsonProcessingException {
        String token = request.getHeader("authorization");
        token = token.substring(7);
        String[] chunks = token.split("\\.");
        Base64.Decoder decoder = Base64.getUrlDecoder();

        String header = new String(decoder.decode(chunks[0]));
        String payload = new String(decoder.decode(chunks[1]));
        return objectMapperDisableUnknownProperties.readValue(payload, TokenMapper.class);
    }
```
### Object mapper cho header
```java
        public HeaderMapper getHeader() throws JsonProcessingException {
        Enumeration<String> headerNames = request.getHeaderNames();
        while (headerNames.hasMoreElements()) {
            String key = (String) headerNames.nextElement();
            String value = request.getHeader(key);
    //        String keyUpdate = key.replace("-", "");
    //        map.put(keyUpdate, value);
            map.put(key, value);
        }
        String json = objectMapperDisableUnknownProperties.writeValueAsString(map);
        return objectMapperDisableUnknownProperties.readValue(json, HeaderMapper.class);
    }
```
### Object mapper hỗ trợ lưu list và parse list lên redis
```java
    List<String> abc = List.of("admin", "member");
    redisTemplate.opsForValue().set("keyOfRedis", objectMapperDisableUnknownProperties.writeValueAsString(abc));


    Object list = redisTemplate.opsForValue().get("keyOfRedis");
    List<String> reciever = objectMapperDisableUnknownProperties.readValue(list.toString(), new TypeReference<>() {});
    // Có thể thay thế String thành 1 list object
```

Ref: https://github.com/NamNV2496/ObjectMapper
