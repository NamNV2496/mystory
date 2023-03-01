# Scheduler

## Mục đích
chạy function theo thời gian quy định

### Chạy lặp với khoảng thời gian fixedRate sau khi đi deploy initialDelay (Sử dụng kết hợp fixedRate và initialDelay)

- fixedRate:
```java
@Scheduled(fixedRate = 2000) // chạy mỗi 2s
    public void scheduleTaskWithFixRate() {
        LOGGER.info("job called!");
    }
```
- initialDelay:
```java
@Scheduled(initialDelay = 10000) // chạy sau khi khởi app startup 10s và chỉ chạy 1 lần
    public void scheduleTaskWithInitialDelay() {
        LOGGER.info("job called!");
    }
```
### Hẹn giờ với cron (Sử dụng cron)
```java
@Scheduled(cron = "15 * * * * ?")
public void scheduleTaskWithCronExpression() {
    LOGGER.info("job called!");
}
```




## Go to FixDelayService.java file to enable scheduler which you want

@ConditionalOnProperty is an annotation to enable/disable the scheduler
```java
    @ConditionalOnProperty(value = "com.scheduling.enabled", havingValue = "false")
```
- havingValue **false** *(this property only: true or false value)* means _com.scheduling.enabled: false_ this condition will pass

- if _com.scheduling.enabled: true_ the condition will false

Reverse, if havingValue is **true** the pass conditional is _com.scheduling.enabled: true_,
false case is _com.scheduling.enabled: false_
