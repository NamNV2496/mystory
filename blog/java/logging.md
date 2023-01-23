# Logging

![img.png](/blog/java/img/logging.jpg)

log là 1 phần rất quan trọng trong dự án, dựa trên log giúp chúng ta debug và kiểm thử. Vậy nên cần 1 quy chuẩn cho việc logging này

## Phân loại

Có 2 loại logging phổ biến:
 - logback
 - sleuth

### Logback cho chúng custom thông tin của log in ra, ví dụ thêm thông tin requestId, hoăc 1 threadId nào đó ( kết hợp MDC để đẩy thông tin ra)

### sleuth config nhanh chóng chỉ cần thêm trong pom mà không cần config nhiều với requestId tự sleuth quản lý



## *WAY 1: logback* 

When a file in the classpath has one of the following names, Spring Boot will automatically load it over the default configuration:

- logback-spring.xml
- logback.xml
- logback-spring.groovy
- logback.groovy

### we wrap `RequestBodyAdviceAdapter` and `ResponseBodyAdviceAdapter` to print log of request and reponse


```text
@ControllerAdvice
public class LogRequestBodyAdviceAdapter extends RequestBodyAdviceAdapter {
    @Autowired
    ILoggingService loggingService;

    @Autowired
    HttpServletRequest httpServletRequest;

    @Override
    public boolean supports(MethodParameter methodParameter, Type type, Class<? extends HttpMessageConverter<?>> aClass) {
        return true;
    }

    @Override
    public Object afterBodyRead(Object body, HttpInputMessage inputMessage, MethodParameter parameter, Type targetType,
                                Class<? extends HttpMessageConverter<?>> converterType) {
        loggingService.logRequest(httpServletRequest, body);

        return super.afterBodyRead(body, inputMessage, parameter, targetType, converterType);
    }
}

@ControllerAdvice
public class LogResponseBodyAdviceAdaper implements ResponseBodyAdvice<Object> {

    @Autowired
    ILoggingService loggingService;

    @Override
    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
        return true;
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter methodParameter, MediaType mediaType,
                                  Class<? extends HttpMessageConverter<?>> aClass, ServerHttpRequest request,
                                  ServerHttpResponse response) {
        if (request instanceof ServletServerHttpRequest && response instanceof ServletServerHttpResponse) {
            loggingService.logResponse(((ServletServerHttpRequest) request).getServletRequest(),
                    ((ServletServerHttpResponse) response).getServletResponse(), body);
        }
        return body;
    }
}


MDC.put("transaction.id", tx.getTransactionId());

add to logback.xml

%X{transaction.id} to catch it
```

log type in `LoggingServiceImpl` class

```text

REQUEST method=[GET] path=[/person] headers=[application/jsonPostmanRuntime/7.29.2*/*58899ae1-4727-43c7-8ff7-01ebdc264586localhost:8080gzip, deflate, brkeep-alive53]

RESPONSE method=[GET] path=[/person] headers=[application/jsonPostmanRuntime/7.29.2*/*58899ae1-4727-43c7-8ff7-01ebdc264586localhost:8080gzip, deflate, brkeep-alive53] body=[Person(id=1, name=nam, age=13)]

```
API:

    GET http://localhost:8080/person

    GET http://localhost:8080/hello

```textmate

%d – outputs the time that the log message occurred in formats that SimpleDateFormat allows.
%thread – outputs the name of the thread that the log message occurred in.
$-5level – outputs the logging level of the log message.
%logger{36} – outputs the package + class name the log message occurred in. The number inside the brackets represents the maximum length of the package + class name. If the output is longer than the specified length, it will take a substring of the first character of each individual package starting from the root package until the output is below the maximum length. The class name will never be reduced. A nice diagram of this can be found in the Conversion Word docs.
%M – outputs the name of the method that the log message occurred in (apparently this is quite slow to use and not recommended unless you're not worried about performance, or if the method name is particularly important to you).
%msg – outputs the actual log message.
%n – line break.
%magenta() – sets the color of the output contained in the brackets to magenta (other colors are available).
highlight() – sets the color of the output contained in the brackets depending on the logging level (for example ERROR = red).
```

# *WAY 2: SLEUTH* ## 

```text
    <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-sleuth</artifactId>
            <version>3.1.4</version>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
config on POM and remove logback-spring.xml

### Note: ### We can't migrate logbak with Sleuth 

[Reference](https://dzone.com/articles/configuring-logback-with-spring-boot)
