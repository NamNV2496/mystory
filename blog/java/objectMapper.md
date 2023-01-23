# Object mapper

Object mapper là 1 cách để convert String/Json sang dạng object. Thông thường thì sẽ dùng để kết hợp đẩy lên redis và lấy thông tin từ redis về. Hoặc parse token

## Cài đặt

```test
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

```text

 private final ObjectMapper objectMapperDisableUnknownProperties;

String json = objectMapperDisableUnknownProperties.writeValueAsString(map);
return objectMapperDisableUnknownProperties.readValue(json, HeaderMapper.class);
 ```



