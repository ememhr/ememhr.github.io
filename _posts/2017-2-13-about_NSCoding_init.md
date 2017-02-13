---
layout: post
title: NSCodingのinitについて
---

# さまり

Jump Right In でNSCodingを初期化する時に required と convenience を使って初期化している意図がわからなかったので調べた話

# NSCodingの初期化について

公式のリファレンス
https://developer.apple.com/reference/foundation/nscoding/1416145-init

```
init(coder:)
Required. Returns an object initialized from data in a given unarchiver.
```

NSCodingのinitの実装箇所にジャンプしても `require` 修飾子はついていなかったものの、公式リファレンスには、`require` が必須と書いてあった。

`NSCoding` は2つのメソッドの宣言を必要とする

1. `encode(aCoder: NSCoder)` → KVOで値を保存（Archive）する
2. `init(aDecoder: NSCoder)` → KVOで保存した値を解凍（UnArchive）する

上の２つのメソッドを宣言することでモデル層でKey Value Object形式であたりをアプリ内に保存します。
