# 如何在运行时获取对象的属性名称

```objc
unsigned int propertyCount = 0;
objc_property_t *properties = class_copyPropertyList([self class], &propertyCount);
    
NSMutableSet *propertyNames = [NSMutableSet set];
for (unsigned int i = 0; i < propertyCount; i++) {
    objc_property_t property = properties[i];
    const char *name = property_getName(property);
    NSString *objcStyleName = [NSString stringWithUTF8String:name];
    [propertyNames addObject:objcStyleName];
}
free(properties);
```

- `propertyNames` 中便包含了对象的属性名称
- 实际上，`propertyNames` 中包含的对象名称只局限在本类中的属性，即不包括父类中的属性
- `NSString *objcStyleName = [NSString stringWithUTF8String:name];` 用于消灭 C-style string converted to Objective-C string 的警告
- 根据文档，`properties` 需要调用 `free` 手动释放内存

---

上述方法只能遍历类自身定义的属性，而其父类却不能进行遍历

通过写一个 Category, 实现遍历自身定义的属性以及父类的属性

```objc
// .h
/**
 获取当前类中的所有属性，属性包括从 aClass 这个父类中继承的
 
 若 aClass 为 nil, 则只返回当前类的属性名称集合，不包括父类

 @return 类的属性集合
 */
+ (NSSet<NSString *> *)propertiesListInheritedFromClass:(Class)aClass;
```


```objc
// .m
@implementation NSObject (Property)

+ (NSSet<NSString *> *)propertiesListInheritedFromClass:(Class)aClass {
    NSMutableSet *properties = [NSMutableSet set];
    
    if (aClass == nil) {
        [self properties:&properties OfClass:[self class]];
    } else {
        Class observing = [self class];
        while ([observing isSubclassOfClass:aClass]) {
            [self properties:&properties OfClass:observing];
            observing = [observing superclass];
        }
    }
    
    return [properties copy];
}

+ (void)properties:(NSMutableSet **)set OfClass:(Class)aClass {
    unsigned int propertyCount = 0;
    objc_property_t *properties = class_copyPropertyList(aClass, &propertyCount);
    
    for (unsigned int i = 0; i < propertyCount; i++) {
        objc_property_t property = properties[i];
        const char *name = property_getName(property);
        NSString *objcStyleName = [NSString stringWithUTF8String:name];
        [*set addObject:objcStyleName];
    }
    free(properties);
}

@end
```

