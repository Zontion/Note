---
draft: true
---


### 跳頁(Navigation)相關

#### 介面建立
>1-1 既有的storyboard點擊想當開頭的viewcontroller，選擇Editor -> Embed in -> Navigation Controller。(ToGo也是先設定這個才用程式去寫)
>1-2 選取Navigation Controller，之後可先把預設的Table View Controller先刪掉，然後從Navigation Controller按住Control拖拉到新的ViewController，然後選root view controller。註：Navigation Controller 包住的第一個ViewController稱為“ root view controller”。
>
>註1：方法2與3產生的畫面上面會產生一navigation bar，若要在上面新增按鈕，要選Bar Button Item（不是選Button）。
>註2：於navigation bar可以設定title(預設只有root view controller可以直接設定)，其他的畫面必須再拉進元件Navigation Item。

---

#### 跳轉頁面帶值
1.有些教學會教拉線，使用segue然後設定identifier，然後
```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if segue.identifier == "jump" {
        if let vc = segue.destination as? View2Controller {
                vc.str = "789"
        }
    }
}
```

但是當你遇到第三個畫面又要跳回第一個畫面並且帶值，線圖就會很亂，另外當專案很大時，如果全部ViewController都寫在同一個storyboard，會有XCode讀取速度很慢，所以會拆分成多個storyboard，那不同的storyboard跳轉也會變成有很多額外的設定要做。
所以推薦使用
```swift
let storyboard = UIStoryboard(name: "Insurance", bundle: nil)
let viewController = storyboard.instantiateViewController(withIdentifier: "Insurance") as! InsuranceViewController
self.navigationController?.pushViewController(viewController, animated: true)
```
這樣Code都能統一寫法，也不用為了跳轉順序一改就要重新拉線或是為了其他storyboard跳轉做額外動作，而且也可以很方便的指定跳轉到別的storyboard的某個指定ViewController。

---

#### Segment與切換畫面
1.介紹哪邊改文字
2.搭配ImageView切換

---

#### CollectionView
1.與UITableView一樣要設定dataSource與delegate。
2.UICollectionView與UITableView差異。
3.設定專屬的cell class
4.CollectionView記得把Estimate設定成none才能有客製化的cell size。

---

#### PageControl
1.先放pagecontrol再放imageView講解視圖覆蓋問題
2.圖片切換講完後 使用搭配動畫呈現(ScrollView + StackView + ImageView達成)
動畫步驟：先用個全版ScrollView，再設定stackview在裡面，同時選取stackview+scrollview的content layout guide(frame layout guide是可見範圍，content layout guide是實際內容大小)並選取四邊Edges = 0，新增imageView到stackView裡面，設定長寬equals scrollView的frame layout guide，記得設定stackView是horizontal且distribution是equals fill，然後新增另外兩個imageView，最後把scrollView設定page enabled(單頁跳轉模式)即可。