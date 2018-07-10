# 向对象添加或读取了不存在的 Key, 如何挽救

实现 `NSObject` 内置的方法

```objc
- (void)setValue:(id)value forUndefinedKey:(NSString *)key {
#if DEBUG
    NSLog(@"%@ is setting undefined key: %@", [self class], key);
#endif
    return;
}

- (id)valueForUndefinedKey:(NSString *)key {
#if DEBUG
    NSLog(@"%@ is reading an undefined key: %@", [self class], key);
#endif
    return nil;
}
```

