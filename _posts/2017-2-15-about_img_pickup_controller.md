---
layout: post
title: PhotoLibraryから画像を選択して画面に描画する実装
---

画像を選択してUIImageに表示させるという処理を実装します。

## 手順

`UIImagePickerControllerDelegate` と `UINavigationViewControllerDelegate` を実装したViewControllerに継承させます。

UIImagePickerControllerDelegate...UIImagePicler → Libraryから画像を取得（pick)するためのViewController
UINavigationControllerDelegate...画像を　取得するControllerに遷移しているのUINavigationViewContorollerを介して（動作としてはUINavigationViewController）いるので、この動作を対象のViewControllerに移譲する。

UIImagePickerControllerDelegateを継承することで、

```swift
func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {}
```

```swift
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {}
```

の２つのメソッドが対象のViewControllerで利用できるようになる。

上記２つのメソッドについては `Cancel` は写真ライブラリから戻るときだとわかりやすい。

`imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any])` のメソッドは `UIImagePickerControllerDelegate` プロトコルの持っているメソッド。

公式リファレンスはこちら  
<a href="https://developer.apple.com/reference/uikit/uiimagepickercontrollerdelegate/1619126-imagepickercontroller
" target="_blank">https://developer.apple.com/reference/uikit/uiimagepickercontrollerdelegate/1619126-imagepickercontroller</a>


第一引数 **picker** ... The controller object managing the image picker interface. ピックアップする動作（interface）  
第二引数 **info** : Dictionary...  
A dictionary containing the original image and the edited image, if an image was picked; or a filesystem URL for the movie, if a movie was picked. The dictionary also contains any relevant editing information. The keys for this dictionary are listed in Editing Information Keys.  

originalImageとeditedImageをdictionaryが持っている。  
JumpRighitInではoriginalimageを使っている。

UIImagePickerControllerOriginalImageの公式リファレンスは以下  
<a href="https://developer.apple.com/reference/uikit/uiimagepickercontrolleroriginalimage" target="_blank">https://developer.apple.com/reference/uikit/uiimagepickercontrolleroriginalimage</a>



```swift
let UIImagePickerControllerOriginalImage: String
```

`UIImagePickerControllerOriginalImage` はImageと付いているが、UIImageを取り出すためのKey。  
infoにKVSでUIImageが格納されている。

infoに保存されているUIImageのdectionaryでvalueを取り出すためのキーの一覧  
<a href="https://developer.apple.com/reference/uikit/uiimagepickercontrollerdelegate/editing_information_keys" target="_blank">https://developer.apple.com/reference/uikit/uiimagepickercontrollerdelegate/editing_information_keys</a>

公式のリファレンス見ると、iOS側のimagePickerControllerで用意されているものが多く、普段使っているアプリ内の画像選択でも似たいような処理になっているところは多いので、イメージしやすかった。
