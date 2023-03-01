# Microservices


## 1 Microservices là gì?

Hiểu theo cách đơn giản thì, microservice là một kiếu kiến trúc phần mềm. Các module trong phần mềm này được chia thành các service rất nhỏ (microservice). Mỗi service sẽ được đặt trên một server riêng -> dễ dàng để nâng cấp và scale ứng dụng.

![img.png](blog/java/img/microservices1.png)

## 2 Monolith Application là gì?

Khi xây dựng phần mềm theo kiến trúc monolith (một khối). Toàn bộ các module (view, business, database, report) đều được gom chung vào một project lớn. Khi deploy, chúng ta sẽ quăng khối code này lên server và config để nó chạy.



![img.png](blog/java/img/microservices2.png)


## Các ưu điểm của Kiến trúc Microservices


- Điều quan trọng nhất là rất dễ nâng cấp và scale up, scale down. Giả sử bạn làm một trang web liên quan tới vận tải, kho bãi. Khi số lượng xe hay hàng hóa tăng lên, chỉ việc nâng cấp server cho service liên quan đến nghiệp vụ kho vận(ngược lại, có thể giảm server nếu cần thiết). Với cloud computing, việc nâng cấp server vô cùng dễ dàng chỉ với vài cú click chuột. Điều này rất khó thực hiện với monolith.
- Do tách biệt nên nếu một service bị lỗi, toàn bộ hệ thống vẫn hoạt động bình thường. Với monolith, một module bị lỗi có thể sẽ kéo sập toàn bộ hệ thống.
- Các service nằm tách biệt nhau, chúng có thể được sử dụng các ngôn ngữ lập trình riêng, database riêng. VD service xử lý ảnh có thể viết bằng C++, service tổng hợp data có thể viết bằng Python.
- Có thể áp dụng được các quy trình tự động hóa, như build, deploy, monitoring,...
Khi chia nhỏ các service, team size sẽ giảm và mọi người sẽ làm việc hiệu quả hơn

## Nhược điểm
- Các module giao tiếp qua mạng nên có thể tốc độ không cao bằng monolith. Ngoài ra, mỗi module phải tự giải quyết các vấn đề về bảo mật, transaction, lỗi kết nối, quản lý log files.
- Việc đảm bảo tính đồng nhất trong dữ liệu sẽ trở nên phức tạp hơn
- Sử dụng nhiều service nên việc theo dõi, quản lý các service này sẽ phức tạp hơn
- Cần một đội ngũ thật ngon để thiết kế và triển khai bao gồm software architect xịn


## Những điều cần tuân thủ khi thiết kế Microservices architecture

- Nguyên tắc của Microservices đó là phải giới hạn phạm vi và tập trung chính vào một nhiệm vụ để hỗ trợ phát triển và triển khai dịch vụ một cách nhanh chóng và hiệu quả nhất.
- Trong khi thiết kế, lập trình viên cần giới hạn phạm vi các services theo nghiệp vụ, chức năng thức tế.
- Cần đảm bảo Microservices có thể thực thi và phát triển độc lập thành từng module riêng.
- Mục tiêu chính của Microservices là sẽ thực hiện hiệu quả nhất có thể những nghiệp vụ đơn lẻ chứ không chỉ cơ bản là thực hiện những dịch vụ nhỏ hơn.
- Kích thước phù hợp của mỗi một service đó là kích thước để đáp ứng đủ những yêu cầu của một nghiệp vụ đơn trong hệ thống.
- Một service không nên sử dụng quá nhiều hàm hay các chức năng hỗ trợ xung quanh cũng như định dạng các thông báo đơn giản.



## 3 Xây dựng microservices trong spring boot như thế nào?


![img.png](blog/java/img/microservices3.png)


**Các component cần thiết** 

- 1 server quản lý hay còn đc gọi là `service register`
- 1 gateway
- 1 load balancer
- 1 authentication service
- 1 cloud config (optional) để ấn giấu các thông tin nhạy cả

### **a, Service register**

Thường người ta hay sử dụng service register sẵn có trong AWS hoặc Kong trong k8s. Nhưng với mục đích làm quen và fee để dễ config ta có thể thử với Eureka Server của netflix với ưu điểm:

- Chúng ta không cần hardcode địa chỉ IP của mỗi microservice mà gọi nó theo tên. Ví dụ gõ  `google.com` thay vì gõ địa chỉ 64.233.181.99 để truy cập vào google.

- Khi mà các service của chúng ta deploy thực thế thì phải sử dụng IP động, Euroka sẽ tự động giúp cập nhật, chúng ta không cần thay đổi code và cũng chỉ gần gọi tên service mà ko cần quan tâm địa chỉ IP của nó


```java
POM

<dependency>  
        <groupId>org.springframework.cloud</groupId>  
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>  
    </dependency>  

    <dependency>  
        <groupId>org.springframework.boot</groupId>  
        <artifactId>spring-boot-devtools</artifactId>  
        <scope>runtime</scope>  
    </dependency>

```

```java
application.properties

spring.application.name=eureka-server

# default port for eureka server
server.port=8761

# eureka by default will register itself as a client. So, we need to set it to false.
# What's a client server? See other microservices (image, gallery, auth, etc).

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```
```java
Config file
@EnableEurekaServer  
```


Trong ví dụ này chúng ta dùng spring-boot-parent `2.6.1` các bản cao hơn có thể ko tương thích

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

access vào `http://localhost:8761` để view GUI của Euroka



![img.png](blog/java/img/microservices4.png)


### **b, Gateway**


Thông thường AWS và k8s đều có sẵn các gateway nên khi sử dụng dịch vụ ta sẽ có sẵn. Vậy free ta nên dùng gì?

có 2 sự lựa chọn:

- cloud gateway (của spring phát triển)
- zuul keeper (của netflix phát triển)


#### **Cloud Gateway**

```java
POM
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>

    <!-- import vì service này cũng cần đăng ký để Euroka quản lý-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>

```
```java
appication.properties

spring.application.name=gateway
# port
server.port=8100
# eureka server url
eureka.client.service-url.default-zone=http://localhost:8761/eureka

# routing
spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.discovery.locator.lower-case-service-id=true

spring.cloud.gateway.routes[0].id=pgService
spring.cloud.gateway.routes[0].uri=http://localhost:2005/
spring.cloud.gateway.routes[0].predicates[0]=Path=/employee/**
spring.cloud.gateway.routes[1].id=inMateService
spring.cloud.gateway.routes[1].uri=http://localhost:2006/
spring.cloud.gateway.routes[1].predicates[0]=Path=/consumer/**
```

```java
config file
@EnableEurekaClient
```


sau khi khởi chạy và có dependency cùng config trỏ vào server Euroka thì server `gateway` sẽ tự động được đăng ký vào Euroka

![img.png](blog/java/img/microservices5.png)

![img.png](blog/java/img/microservices6.png)


#### **Zuul keeper Gateway**

lợi thế của zuul keeper là người tạo ra nó là netflix và được bảo đảm về performance cũng như dễ config routing hơn. Trong công nghệ sử dụng kafka cũng đang sử dungh Zuul keeper làm gateway

```java
POM
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
            <version>2.2.9.RELEASE</version>
        </dependency>
```

```java
appication.properties

spring.application.name=zuul-server
server.port=8082
eureka.client.service-url.default-zone=http://localhost:8761/eureka/
# A prefix that can added to beginning of all requests.
#zuul.prefix=/api
# Disable accessing services using service name (i.e. gallery-service).
# They should be only accessed through the path defined below.
zuul.ignored-services=*
# Map paths to services
zuul.routes.image-service.path=/image/**
zuul.routes.image-service.url=http://localhost:8000
ribbon.eureka.enabled=true
```

```java
config file
@EnableEurekaClient
@EnableZuulProxy
```

![img.png](blog/java/img/microservices7.png)



### **c, Service con**

cần config để có thể đăng ký vào Euroka

```java

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>



spring.application.name=gallery-service  
server.port=8100  
eureka.client.service-url.default-zone=http://localhost:8761/eureka


@EnableEurekaClient  
```


### **d, Load balancer**

Đây là 1 con giảm tải và phân tải cho các instance. Thông thường có 2 loại:
- hardware load balancer
- software load balancer

và chúng thường có sẽ trong các dịch vụ của AWS hay K8s


Với Euroka server, chúng sử dụng Ribbon

    ribbon.eureka.enabled=true


### **e, Cloud config**


ở service cloud config
```java
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>

    <!-- config để lưu trên github-->
    spring.cloud.config.server.git.uri=https://github.com/NamNV2496/-spring-cloud-config
    spring.cloud.config.server.git.uri.username=XXXX
    spring.cloud.config.server.git.uri.password=XXXX
```

ở các service load file config của cloud config này

```java
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>

    spring:
        cloud:
            config:
            uri: http://localhost:8888
```

Annotation `@RefreshScope` là 1 annotation của spring cloud. Các Bean được đánh dấu với annotation này sẽ được làm mới tại thời gian chạy (runtime). Mỗi khi gọi tới thì nó sẽ tạo 1 bean mới.




