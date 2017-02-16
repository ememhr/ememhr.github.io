---
layout: post
title: UIStackViewの削除に関しての追加学習
---

以前書いた[カスタムUIStackViewクラス内の子要素を削除する](https://ememhr.github.io/RemoveUIStackView/)の追記

1. removeArrangedSubview()
2. (UIView).removeFromSuperView

の動作の違いとなぜ2つのメソッドを使わないと完全に削除できないのかということについて。

removeArrangedSubview  
<a href="https://developer.apple.com/reference/uikit/uistackview/1616235-removearrangedsubview" target="_blank">https://developer.apple.com/reference/uikit/uistackview/1616235-removearrangedsubview</a>

removeFromSuperview  
<a href="https://developer.apple.com/reference/uikit/uiview/1622421-removefromsuperview" target="_black">https://developer.apple.com/reference/uikit/uiview/1622421-removefromsuperview</a>


removedArrengedSubView addArrangedSubviewで追加したviewのみ削除する。しかし公式リファレンスによると

```
However, this method does not remove the provided view from the stack’s subviews array; therefore, the view is still displayed as part of the view hierarchy.
```

stack's subview arrayは削除できない。
この stack's subview array が残る。

そもそも stack's subview array とは何か？

removeArrangedSubview(削除対象UIView)メソッドが動作して削除するとき  

stack's sarrangedSubview → 削除できる。
stack's subview → 削除できない。

という動作がある。

1. arrangedSubview → <a href="https://developer.apple.com/reference/uikit/uistackview/1616235-removearrangedsubview" target="_blank">https://developer.apple.com/reference/uikit/uistackview/1616235-removearrangedsubview</a>
2. subview → <a href="https://developer.apple.com/reference/uikit/uiview/1622614-subviews" target="_blank">https://developer.apple.com/reference/uikit/uiview/1622614-subviews</a>

arrangedSubviews は subview の 配列の 集合体
add した時は stack view が subview としてこの arrangedSubview を追加する  
remove した時は stack view が arrangedSubview の配列を削除する。  
このとき、もともとの stack view の subview は削除しない。  
階層構造として、stackview に紐づくarrangedSubview は削除するけど、stackview に紐づく subview は削除してくれない。  

```
stackview
  subview
    arrangedSubview
```

という階層構造になっているのだと思う。storyboard上に出てこないのでわからない...  
なので、removeArrangedSubviewした時に stackview 直下の subview は残るので、このsubview も removeFromSuperView() メソッドで参照を切り離さないといけない。

removeArrangedSubview(削除対象UIView) の後に (削除対象UIView).removeFromSuperView を使うのは公式リファレンスでも書かれているので、一般的な方法なのだと思う。

```
To prevent the view from appearing on screen after calling the stack’s removeArrangedSubview: method, explicitly remove the view from the subviews array by calling the view’s removeFromSuperview() method, or set the view’s isHidden property to true.
```

(削除対象のUIView).removeFromSuperview or (削除対象UIView).isHidden = true で残っているStackview　を削除 or 非表示にする必要がある。

