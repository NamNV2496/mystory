# **Asynchronous (bất đồng bộ)**

## Không phải ngẫu nhiên đây là các câu hỏi dành cho senior khi tham gia phỏng vấn. Thực sự async là 1 vấn đề khá khó và cần kinh nghiệm để xử lý

### Đầu tiên thì đặt vấn đề: Nhiều người nhầm lẫn giữa đa luồng (multithread) và bất đồng bộ (asynchoronous). Cùng phân biệt nó nhé

## **Asynchoronous (bất đồng bộ)**
- Mô hình bất đồng bộ cho phép nhiều thứ xảy ra cùng lúc khi chương trình của bạn gọi 1 long-function cần nhiều thời gian để thực hiện. và đặt biệt `nó không block chương trình lại` và khi hoàn thành chương trình sẽ biết và lấy về giá trị kết quả nếu cần 

![img.png](blog/java/img/asynchoronous2.png)
![img.png](blog/java/img/asynchoronous19.png)

- Một chương trình bất đồng bộ là tạo các chức năng thực hiện hành động chậm và có thêm một đối số, một hàm gọi lại (callback). Hành động được bắt đầu và khi nó kết thúc, chức năng gọi lại được gọi với kết quả hoặc không. Async có thể xảy ra tại nhiều thread (thường là các thread để lắng nghe callback để thực hiện tiếp tục) => nên thường bị nhầm sang multithread

Nếu muốn dùng để trả về main thread sau khi có kết quả thì sử dụng `CompletedFuture`

![img.png](blog/java/img/asynchoronous14.png)

result:

![img.png](blog/java/img/asynchoronous16.png)

if use `@Async` or ` CompletableFuture.supplyAsync()` then it will handle in another thread

![img.png](blog/java/img/asynchoronous17.png)
result:
![img.png](blog/java/img/asynchoronous15.png)

## **Multithread (đa luồng)**
- MultiThreading đề cập đến việc thực hiện đồng thời/song song của nhiều hơn một bộ tuần tự (luồng) của các câu lệnh.

![img.png](blog/java/img/asynchoronous5.png)


![img.png](blog/java/img/asynchoronous20.png)


# **Ví Dụ**

- **Asynchoronous (bất đồng bộ)**

![img.png](blog/java/img/asynchoronous11.png)

- **Multithread (đa luồng)**

![img.png](blog/java/img/asynchoronous12.png)



# **Which One To Use?**
In a nutshell, for large scale applications with a lot of I/O operations and different computations, using asynchronous multithreading programming flow, will utilize the computation resources, and take care of non-blocking functions. This is the programming model of any OS!

![img.png](blog/java/img/asynchoronous18.png)


![img.png](blog/java/img/asynchoronous10.png)

**=>** Lập trình đa luồng là tất cả về thực hiện đồng thời các chức năng khác nhau. Lập trình bất đồng bộ là về thực thi không chặn giữa các chức năng.


## Enable async config

`@EnableAsync`

create a bean with special name: `asyncExecutor`

![img.png](blog/java/img/asynchoronous4.png)

with data:

![img_1.png](blog/java/img/asynchoronous1.png)


Call async custom config by:

![img_9.png](blog/java/img/asynchoronous9.png)

    GET http://localhost:8080/test2 

![img_3.png](blog/java/img/asynchoronous3.png)

# Use EAAsync

![img_6.png](blog/java/img/asynchoronous6.png)

to run with async and handle the return value. We use `thenApply()`

![img_7.png](blog/java/img/asynchoronous7.png)

    GET http://localhost:8080/eaAsync

![img_8.png](blog/java/img/asynchoronous8.png)



To merge future of 2 thread

```java
              @Async
              Service A(thread1,thread2) \
MicroService /                             (Merge from Response of ServiceA and ServiceB)
             \ @Async
              Service B(thread1,thread2) /
```

```java
You need to use spring's AsyncResult class to wrap your result and then use its method .completable() to return CompletableFuture object.

When merging future object use CompletableFuture.thenCompose() and CompletableFuture.thenApply() method to merge the data like this:

CompletableFuture<Integer> result = futureData1.thenCompose(fd1Value -> 
                futureData2.thenApply(fd2Value -> 
                        merge(fd1Value, fd2Value)));
```

https://stackoverflow.com/questions/60538451/spring-boot-async-with-multithreading


Ref: https://viblo.asia/p/lap-trinh-da-luong-voi-completablefuture-trong-java-8-6J3ZgBMLKmB