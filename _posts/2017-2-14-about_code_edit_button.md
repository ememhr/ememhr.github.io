---
layout: post
title: コードからTableViewのeditボタンを生成する
---

UINavigationViewControllerをEnbedInしている状態で編集ボタンをコードから生成する。

```swift
navigationItem.leftBarButtonItem = editButtonItem
```

これで該当NavigationControllerを設置している箇所に対して、左上に編集ボタンを配置できる。

また、TableViewControllerで、

```swift
// Override to support conditional editing of the table view.
override func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
    // Return false if you do not want the specified item to be editable.
    return true
}

// Override to support editing the table view.
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        // Delete the row from the data source
        tableView.deleteRows(at: [indexPath], with: .fade)
    } else if editingStyle == .insert {
        // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
    }
}
```

上記箇所のコメントアウトを外す。  

```swift
// Override to support conditional editing of the table view.
override func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
    // Return false if you do not want the specified item to be editable.
    return true
}
```

編集可能にする上記はTrueにする。
