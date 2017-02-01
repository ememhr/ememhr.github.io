---
layout: post
title: カスタムUIStackViewクラス内の子要素を削除する
---

## UIStackViewとは？
iOS9から導入された概念  
UIView郡をUIStackView内に入れ子として管理できる。  
UIStackViewが親でUIButtonなどの各UIパーツが子要素になる。  

## Apple公式チュートリアル内でボタンを削除する

[JumpRightIn](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/ImplementingACustomControl.html#//apple_ref/doc/uid/TP40015214-CH19-SW1)  

対象となるコードはこちら(抜粋)  

```swift

private var ratingButtons = [UIButton]()

// clear any existing buttons
for button in ratingButtons {
    removeArrangedSubview(button)
    button.removeFromSuperview()
}
ratingButtons.removeAll()

```

## UIStackViewの削除

メソッドの一つ一つ動作

1. `removeArrangedSubView` でUIStackViewに割り当てられているUIパーツの配列から該当UIパーツを削除する
1. `burron.removeFromSuperview` でSuperViewからUIButtonの参照を外す
1. `ratingButtons.removeAll()` で配列を空にする
   
`arrangedSubview` はSubview(ここではUIStackView)に組み込まれたViewのこと。公式チュートリアルでは星型のUIButtonがこれにあたる。  
この`arrangedSubview` は中に配列としてUIパーツを持つので、最初に対象のUIパーツをこの配列から外す。  
次に、`removeFromSuperView` で対象のUIパーツの参照を親のViewから外す。

これで参照されるUIパーツがUIViewから削除されて新しくボタンを定義できるようになる。

## 参照 メソッドの意味
- <a  href='https://developer.apple.com/reference/uikit/uiview/1622421-removefromsuperview' target='_blank'>removeFromSuperView</a>
- <a href='https://developer.apple.com/reference/uikit/uistackview/1616235-removearrangedsubview' target='_blank'>remove_rrangedSubView</a>
- <a href="https://developer.apple.com/reference/uikit/uistackview/1616232-arrangedsubviews" target="_blank">arrangedSubView</a>
