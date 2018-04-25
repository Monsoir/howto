# 如何在 text view 中添加 Placeholder

```swift
import UIKit

private var NeedPlaceholderContextKey: UInt8 = 0
private var PlaceholderContextKey: UInt8 = 1
extension UITextView: UITextViewDelegate {
    private var needPlaceholder: Bool {
        get {
            return associatedValue(base: self, key: &NeedPlaceholderContextKey, initialiser: { () -> Bool in
                return false
            })
        }
        
        set {
            associateValue(base: self, key: &NeedPlaceholderContextKey, value: newValue)
        }
    }
    
    private var placeholder: String {
        get {
            return associatedObject(base: self, key: &PlaceholderContextKey, initialiser: { () -> String in
                return ""
            })
        }
        
        set {
            associateObject(base: self, key: &PlaceholderContextKey, value: newValue)
        }
    }
    
    // 通过这个属性来获取文本，否则可能会获取到 placeholder 的值
    var realText: String! {
        get {
            if (self.needPlaceholder && self.textColor == .lightGray) {
                return ""
            } else {
                return self.text
            }
        }
    }
    
    convenience init(placeholder: String) {
        self.init()
        setPlaceholder(placeholder: placeholder, shouldSet: true)
        self.textColor = .lightGray
        self.text = placeholder
    }
    
    func setPlaceholder(placeholder: String = "", shouldSet: Bool = false) {
        needPlaceholder = shouldSet
        if (shouldSet) {
            self.placeholder = placeholder
            delegate = self
        } else {
            self.placeholder = ""
            delegate = nil
        }
    }
    
    public func textViewDidBeginEditing(_ textView: UITextView) {
        if (self.needPlaceholder && textView.textColor == .lightGray) {
            textView.text = nil
            textView.textColor = .black
        }
    }
    
    public func textViewDidEndEditing(_ textView: UITextView) {
        if (self.needPlaceholder && textView.text.isEmpty) {
            textView.text = self.placeholder
            textView.textColor = .lightGray
        }
    }
}
```

