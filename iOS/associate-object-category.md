# 如何在 category 中关联对象


## Objective-C

```objc
#import <objc/runtime.h>
```

```objc
@interface NSObject (AssociatedObject)
@property (nonatomatic, strong) id associatedObject;
@end
```

```objc
@implementation NSObject (AssociatedObject)
@dynamic associatedObject; // 告诉编译器不需要生成默认的 setter 和 getter，runtime 时实现

//static char associatedObjectKey; //其实最好用这个东西来表明地址

- (void)setAssociatedObject:(id)object{
		objc_setAssociatedObject(self, @selector(associatedObject), associatedObject, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
		
		//objc_setAssociatedObject(self, &associatedObjectKey, associatedObject, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

- (id)associatedObject{
		return objc_getAssociatedObject(self, @selector(associatedObject));
		
		//return objc_getAssociatedObject(self, &associatedObjectKey);
}
@end
```

## Swift

```swift
import Foundation

// for 数据类型
func associatedValue<ValueType: Any>(base: AnyObject, key: UnsafePointer<UInt8>, initialiser: () -> ValueType) -> ValueType {
    if let associated = objc_getAssociatedObject(base, key) as? ValueType { return associated }
    let associated = initialiser()
    objc_setAssociatedObject(base, key, associated, .OBJC_ASSOCIATION_ASSIGN)
    return associated
}

func associateValue<ValueType: Any>(base: AnyObject, key: UnsafePointer<UInt8>, value: ValueType) {
    objc_setAssociatedObject(base, key, value, .OBJC_ASSOCIATION_ASSIGN)
}

// for 对象类型
func associatedObject<ValueType: Any>(base: AnyObject, key: UnsafePointer<UInt8>, initialiser: () -> ValueType) -> ValueType {
    if let associated = objc_getAssociatedObject(base, key) as? ValueType { return associated }
    let associated = initialiser()
    objc_setAssociatedObject(base, key, associated, .OBJC_ASSOCIATION_RETAIN)
    return associated
}

func associateObject<ValueType: Any>(base: AnyObject, key: UnsafePointer<UInt8>, value: ValueType) {
    objc_setAssociatedObject(base, key, value, .OBJC_ASSOCIATION_RETAIN)
}

// source: http://swift.gg/2016/10/11/swift-extensions-can-add-stored-properties/
```

### 例子

```swift
private var DxContextKey: UInt8 = 1

extension UITextField {
    var dx: CGFloat {
        get {
            return associatedValue(base: self, key: &DxContextKey, initialiser: { () -> CGFloat in
                return 0
            })
        }
        
        set {
            associateValue(base: self, key: &DxContextKey, value: newValue)
        }
    }
}
```


