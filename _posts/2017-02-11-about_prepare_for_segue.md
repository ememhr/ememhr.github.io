---
layout: post
title: UIViewControllerのprepareメソッドについて
---

# さまり

1. prepareメソッドが理解できずにいた件
2. swiftの外部引数と内部引数について

# prepareメソッドが理解できずにいた件
prepare(for: ,sender:)メソッドがなかなか理解できずにいて、ようやく理解したので、その備忘録です。

まずprepare(for: ,sender:)について公式のリファレンスでは以下のように記載されています。  
<a href="https://developer.apple.com/reference/uikit/uiviewcontroller/1621490-prepare" target="_blank">公式リファレンス</a>

> prepare(for:sender:)  
Notifies the view controller that a segue is about to be performed.

```swift
func prepare(for segue: UIStoryboardSegue, sender: Any?)
// segue ... The segue object containing information about the view controllers involved in the segue.
// sender ... The object that initiated the segue. You might use this parameter to perform different actions based on which control (or other object) initiated the segue.
```

訳すと、segueが動作することをViewControllerに通知するということ。  
取る引数については  
segue : 動作するsegue  
sender : 次のViewControllerに送信するObject(Any?だからなんでも送れる)  

んで何がわからなかったかというと、メソッドのリファレンスにある `(for segue: UIStoryboardSegue, sender: Any?)` っていうかしょ。  
`for` ってなに？っていうのが最初に抱いた疑問でした。

実装コードの中で親クラスの `prepare` メソッドを呼ぶ時に

```swift
super.prepare(for: segue, sender: sender)
```

って記述があって、最初のほう、頭混乱してました。  
で結果なんですが、これ外部引数と内部引数なんですね。  

# 外部引数と内部引数について

swiftはメソッドの呼び出し時に使用する外部引数と、メソッドの内部で利用する内部引数の2つを持つことができます。  
外部引数と内部引数を分けるためには、 `外部引数名 内部引数名 : 型` と書き方をします。  
prepareメソッドで `prepare(for segue: UIStoryboardSegue, sender(Any?))` と記載されていた箇所ですが、

for : 外部引数  
segue : 内部引数  

だったわけです。そのため、swiftでメソッドをドキュメント等で記載する時に `prepare(for:,sender:)` という書き方をしていたわけです。  
ドキュメント等への書き方の場合は、外部引数を使います。  
外部引数と内部引数がわかると、`segue` という変数はメソッド内部で変数の振る舞いがわかりやすいから内部引数と外部引数で分けています。  
swiftでは可読性を上げるために、外部引数と内部引数を併記する場合があります。

例えば、ユーザーを特定のグループに招待するというメソッドを考え見ると

```swift
func invite (user : String, to group: String) {
	print("\(user)を\(group)に招待します。")
}
```

となります。
この場合このメソッドをコールするときには

```swift
invite(user: "Taro", to: "Swift")
```

となり、誰をどのグループに呼んだのか、メソッドからわかり、可読性が上がります。  
ただし、inviteメソッド内部では、変数が `to ` のままではメソッド内部の振る舞いがわかりづらいので、メソッド内部では `group` という変数を使ってメソッド内部での可読性もあげています。

使うと非常に便利ですが、まだ、外部引数と内部引数を分けたコードについて、読み慣れていないので、prepareメソッドの理解の部分でハマりました。
