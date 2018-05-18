# 如何向类中动态添加方法？

来自 AFNetworking 的源码实现

```objc
static inline BOOL af_addMethod(Class theClass, SEL selector, Method method) {
    return class_addMethod(theClass, selector,  method_getImplementation(method),  method_getTypeEncoding(method));
}
```

