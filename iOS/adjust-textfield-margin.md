# 如何调整 text field 的边缘间距

通过继承

```swift
// IndentedTextField.swift

class IndentedTextField: UITextField {
    var dx = CGFloat(10)
    var dy = CGFloat(10)
    
    // for placeholder
    override func textRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.insetBy(dx: dx, dy: dy)
    }
    
    // for text
    override func editingRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.insetBy(dx: dx, dy: dy)
    }
}
```

