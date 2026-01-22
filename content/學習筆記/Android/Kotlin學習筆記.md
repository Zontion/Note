---
draft: true
---


## Day1
## ch1~ch2
1. Kotlin function可以不用寫在class底下 top level function 任何地方都可以呼叫
2. 如果function只有一行的回傳值 可以直接==fun xxx(): String = "123"==
3. 字串中要帶變數用$ ex: return =="1321\$aaa"== (可"1321${運算式}")       以前的==return "1321" + aaa==也可但不好 記憶體空間使用浪費
4. java的三元運算子==x > y ? x : y== Kotlin沒有 但可用**if else** ex:==val v = if (x > y) x else y==
5. Kotlin沒有switch 有 when功能一樣但是寫法不同 且可判斷的資料型態更多
6. Kotlin的for迴圈 多了range的概念 ex: ==for(index: Int in 1\.\.5 step 2)==
7. Kotlin有Array型態 有IntArray這種 定義明確的Array  就可以有==for(n in intArrayX)==
8. class不加open 就不能被繼承  class裡的fun 不加open就不能被覆寫
9. Kotlin沒有new, extends, implements這些key word
10. Kotlin的class可以帶參數 視為constructor ex: ==class Employee(name: String): Person(name) {}==  **子類別繼承時一定要帶父類別的constructer**

## ch3 
11. 一行想要寫多敘述 可以用";"但是幾乎不會這樣做

## ch4 Type
12. Java的int是key word Kotlin的Int是class
13. 數字可以加底線 方便閱讀用 val v1 = 1_000_000
14. Kotlin的型態都是物件類別(Double, Float, Int, Byte...等)所以不能直接指定給不同型態 需要用toByte
15. ***Int型態的321 二進位是0000_0000_0000_0000_0000_0001_0100_0001 轉換成byte型態後 會是剩最後 0100_0001 也就是65 IDE不會報錯
16. Kotlin的boolean不能用0,1
17. Java的String比較用equals Kotlin則是== 且Kotlin字串可以用大於(>)小於(<) ex: "A" > "Z" 是false 比較Unicode大小
18. 字串大寫或小寫的true與false可以toBoolean() (除了true呼叫toBoolean其他都會回傳false)
19. 任何數學運算 計算結果都是預設Int 就算是Byte+Byte答案也會是Int 但是有比Int大的(Long, Double)則會是取大的
20. ==當用Int計算 且結果超過Int 不會報錯 但是答案會是錯的(可能會是負數)==

## ch5 判斷式
21. 建立物件可以直接用=if else if else 或是 =when... 但要記得加else不然不能compile
22. when也可以後面不加() 由下面的各行去寫判斷式 (表達式Epression)

## ch6 Range
23. range 有 in x\.\.y(等同x.rangeTo(y)) step(隔) until(x\.\.y但不包含y) downTo(由大到小)   range只適用(Int, Long, Char(Unicode))
24. in也可以當作判斷式 判斷某變數 是否在某range(陣列物件)裡

## ch7 Array
25. array.indices 就會取得一個range物件(==不過是取index 不是取值== ex:0\.\.2)
26. 除了基本八種類型Array(Byte, Short, Int, Long, Float, Double, Char, Boolean) 其他都是用Array<T> (String或其他類別)
27. 類別陣列可用val array: Array<T> = arrayOfNulls<T>(n)建立 全null陣列  也可以用val names = arrayOf("Tom", "John", "Sam")
28. Kotlin的二維陣列只能 陣列塞陣列 ex: ==val numberList = arrayOf(IntArray(6), IntArray(2))== 或 ==arrayOf(arrayOf(1, 2, 3), arrayOf(1))==

## Day2
## ch8 字串
29. 字串用三個雙引號"""XXX"""會保留中間所有的換行及任何特殊符號,排版
30. StringBuilder在+一萬個"*"的表現下 高於String串接 快100倍
31. StringBuilder本身也提供很多變換字串的function
32. 格式化(format)的各種特殊代碼很好用
33. StringBuffer用在異步工作時比較好管理記憶體位置

## ch9 物件與null
34. 除了基本資料型態以外 物件變數都可以指定null值
35. NullPointException -> ==NPE== 網路很多流行(容易發生)的東西都會變成縮寫
36. 加了問號後允許null值 但是在呼叫function會被IDE要求增加null判斷或是!!強制使用
37. name?.length意思就是name非null則呼叫 name是null則function回傳null
38. name?.length ?: 0 若是null則回傳0
39. nullable的字串 在呼叫isNullorBlank()與isNullOrEmpty時Kotlin不會強制做null判斷

## ch10 函式
41. 沒回傳也能寫 ==:Unit==
42. 參數可以設定預設值 呼叫時就可以不用都帶 也可以指定參數名稱 不照順序帶參數 
43. Java可用public void sum(==int... nums==)來不限制帶多少參數 Kotlin在參數前加==vararg== 即可接受0到多個參數(一個function只能有一個vararg)

## ch11 類別與物件
44. ***class 名稱 constructor(屬性變數宣告...) 要在主建立物件同時建立屬性則需加上==val or var ex: class Person constructor(val id: Long, var title: String)== 若沒加則只視為純參數 且需在下面另外寫每個屬性建立(同Java)
45. constructor也可以在參數欄寫預設值 = n
46. 有寫init{}的話 在建立好物件後 會先跑init 通常是檢查屬性值
47. init中可以使用require做屬性檢查 在設定錯誤時會跳Exception並顯示自己設定的錯誤訊息
48. 若除了建立時的constructor外 還想另外寫不同constructor是可以的 但是須回傳:this(...)  ex: constructor(id: Long): this(id, "", "") {}不過因為主要建立可以 設定預設值 所以不太會用到第二種constructor
49. Kotlin getter setter不另外寫 可以直接person.name呼叫 執行上會自動轉換為呼叫setter & getter
50. 可在屬性底下寫set or get時要執行的動作 裡面建議使用field關鍵字 set用field = n get用return n
51. 有些API為final 不可繼承 可以用**extension**增加額外function或屬性 且可以設定get set ex: ==fun Array<String>.total(): Int== 或 ==val Array<String>.total: Int get(){}==

## ch12 繼承與函式覆寫
52. ==Kotlin預設不能被繼承== class前面加**open**才能被繼承 fun也是(==Java預設可以== 除非加**final**)
53. 從父類別==繼承的參數 不能用var val==額外新增的才可以 子類別建立時也要指定呼叫父類別的constructor ex:class ImageItem(id: Long, val image: String): Item(id)
54. 子類別呼叫父類別fun還是用==super==

## ch13 多型
55. val person: 父類別 = 子類別(...)
56. 好處是Array<父類別>可以放各種子類別元素
57. 判斷是什麼類別可以用==is== ex: ${person is 子類別} 但==所有子類別is父類別 還是true==所以判斷順序要注意
58. 使用is判斷後Kotlin會有智慧轉型效果 可以呼叫到屬於該類別的方法(Java要強制轉型)
59. 若遇到需要自己轉型 可以用as

---

## Day3 Android課程開始
## ch4 Androidg設計
1. AppWidget安裝在桌面的特殊元件 ex:桌面的時鐘或音樂播放列
2. 一個App可以有多個Module以支援同App不同裝置平台(Mobile, Wear)

## ch5 活動元件
3. 畫面設計參考工具 Visual Paradigm Online
4. Log.d d=debug, e = error, v = verbose, w = warn 可在Logcat選單看到

## ch6 元件
5. ViewGroup是View的子類別 ViewGroup是所有Layout的父類別
6. TextView可用android:autoLink="all"讓文字有超連結功能

## ch7 布局
7. LinearLayout只有一個設定weight=1則會佔滿剩下的空間
8. TableLayout span是佔幾格 colum是設定在哪個位子

## ch8 使用者互動
9. 可直接在xml android:onClick="方法名稱"
10. 在Kotlin中setOnClickListener這種只會有一種implement物件且abstract方法只有一個onClick時 都可以使用lambda方式簡寫 ex: button.setOnClickListener{v -> //do something}若用不到v 甚至可以把v -> 省略
11. 用lazy指定給變數 Kotlin不會馬上做 等程式使用到該變數 才會去做lazy
12. onXXXListener() 基本都會有v: View
13. 用不到的變數 可以用_代替 ex:view.setOnFocusChangeListener{_, hasFocus -> //TODO}

## ch10 Res
15. 有android.開頭的資源都是系統內建
16. 可在資源設定orientation, locale...等資源 供app在不同狀況顯示(strings也可以橫向)

## ch11 Activity與互動
17. manifest activity的android:name前面用.代表跟manifest最上面package同路徑
18. manifest activity的android:label是左上角title顯示文字
19. 可用android:screenOrientation設定該activity是垂直還是水平(沒有全app一次設定)
20. Kotlin的intent中要用::class.java
21. app出現詢問用哪個系統開啟檔案 就是action的一種
22. action名稱可以設定多個(通常用在給別的app呼叫用 開啟電話,地圖就是action的一種)
23. 同畫面不同作業(新增&修改) 可以用action來做區分判斷
24. Java的getIntent在Kotlin可以直接呼叫intent來用(Activity的屬性)
25. startActivityForResult可以用requestCode來做區分回傳時做不同事
26. intent帶this或this@差別只在於 如果在巢狀class this會不確定是用哪個
27. permission有分normal()跟dangerous(電話,照片,GPS耗電...等)兩種 

## ch12 RecyclerView & CardView
28. ViewHolder包裝處理所有項目的view物件

## ch13 Fragment
29. 可做到手機與平板不同畫面顯示 或是直立 橫立

## ch15 存取設定資訊
30. 畫面轉向的Lifecycle -> ==onPause==>==onStop==>==onDestroy==>==onCreat==>==onStart==>==onResume==
31. 畫面旋轉時可用==onSaveInstanceState==保存Activity狀態 並在需要的時候讀出


