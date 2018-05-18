# 在正确的线程中执行任务

来自 SDWebImage 的实现

### 在正确的线程中调度任务

```objc
#ifndef dispatch_queue_async_safe
#define dispatch_queue_async_safe(queue, block)\
    if (strcmp(dispatch_queue_get_label(DISPATCH_CURRENT_QUEUE_LABEL), dispatch_queue_get_label(queue)) == 0) {\
        block();\
    } else {\
        dispatch_async(queue, block);\
    }
#endif
```

### 确保在主队列中调度应在主线程运行的任务

```objc
#ifndef dispatch_main_async_safe
#define dispatch_main_async_safe(block) dispatch_queue_async_safe(dispatch_get_main_queue(), block)
#endif
```

