

# Swift學習
### CoreData
1. Editor > Add Model Version... > *** 2.xcdatamodel
2. 切換右邊的Model Version上拉選單 改為新的core data (*** 2)
3. 在AppDelegate修改NSMigratepersistentSotresAutomaticallyOption相關程式

>[!info]
**關鍵字:** coredata version 版本遷移

資料參考: http://cherng32.blogspot.com/2016/07/swift-core-data.html


### Navigation相關

1.介面建立

>1-1 從Button拉線，選Action Segue -> Show(預設跳頁會是卡片式由下往上蓋)。
>1-2 既有的storyboard點擊想當開頭的viewcontroller，選擇Editor -> Embed in -> Navigation Controller。(ToGo也是先設定這個才用程式去寫)
>1-3 選取Navigation Controller，之後可先把預設的Table View Controller先刪掉，然後從Navigation Controller按住Control拖拉到新的ViewController，然後選root view controller。
>
>註1：Navigation Controller 包住的第一個ViewController稱為“ root view controller”。
>註2：方法2與3產生的畫面上面會產生一navigation bar，若要在上面新增按鈕，要選Bar Button Item（不是選Button）。
>註3：於navigation bar可以設定title(預設只有root view controller可以直接設定)，其他的畫面必須再拉進元件Navigation Item。

---

### 橫向CollectionView自動計算Cell寬度
1.ViewController建立UICollectionViewFlowLayout:

```swift
@IBOutlet weak var collectionLayout: UICollectionViewFlowLayout! {
    didSet {
        collectionLayout.estimatedItemSize = UICollectionViewFlowLayoutAutomaticSize
    }
}
```
2.將StoryBoard上目錄中的UICollectionView附帶的CollectionViewFlowLayout(黃黃的那個)拉線至1.
3.在UICollectionViewCell.swift新增
```swift
override func awakeFromNib() {
    super.awakeFromNib()
        
    contentView.translatesAutoresizingMaskIntoConstraints = false
    NSLayoutConstraint.activate([
        contentView.leftAnchor.constraint(equalTo: leftAnchor),
        contentView.rightAnchor.constraint(equalTo: rightAnchor),
        contentView.topAnchor.constraint(equalTo: topAnchor),
        contentView.bottomAnchor.constraint(equalTo: bottomAnchor)
    ])
}
```

