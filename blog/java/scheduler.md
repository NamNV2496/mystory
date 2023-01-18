# Scheduler


## Go to FixDelayService.java file to enable scheduler which you want

@ConditionalOnProperty is an annotation to enable/disable the scheduler

    @ConditionalOnProperty(value = "com.scheduling.enabled", havingValue = "false")

- havingValue **false** *(this property only: true or false value)* means _com.scheduling.enabled: false_ this condition will pass

- if _com.scheduling.enabled: true_ the condition will false

Reverse, if havingValue is **true** the pass conditional is _com.scheduling.enabled: true_,
false case is _com.scheduling.enabled: false_
