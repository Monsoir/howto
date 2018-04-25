# 如何调整 text view 的边缘间距

直接设置属性 `textContainerInset` 即可

```swift
lazy var tvDetail: UITextView = {
    let view = UITextView()
    view.tintColor = .red
    view.font = UIFont.systemFont(ofSize: 20)
    view.textContainerInset = UIEdgeInsetsMake(8, 8, 8, 8) // text view 改变内容 inset
    return view
}()
```

