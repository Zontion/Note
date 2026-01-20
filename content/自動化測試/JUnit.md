
本篇學習還是以JUnit 4為主。
# 簡介
Java / Kotlin最常用的單元測試(Unit Test)框架，跑在JVM上，速度最快、最單純的邏輯測試工具。

# JUnit版本差異

>[!Note]
>JUnit 4 - 使用Java5+，Android Instrumented Test原生支援最好、最相容(爲預設Runner)，生態成熟、文件與範例較多。
>JUnit 5 - 使用Java8+，功能較新、語法更乾淨，Kotlin+Android越來越多採用JUnit 5。
>
>JUnit 4與JUnit 5最明顯差異就是Annotation的名稱也都不相同，ex: JUnit 5將所有的 `@Before` 換成 `@BeforeEach`，所有的 `Asserts` 換成 `Assertions`。

# 使用方式
主要設定在src/test/java/com.example.app/目錄下
新專案預設會有ExampleUnitTest這個class
右鍵檔案>Run即可啟動(或是右上角的綠色箭頭)

JUnit主要是經由Annotation的方式來驅動、控制測試流程。

#### 常見的Annotation
1. <span class="color_yellow_FFD306">@Before</span> - 在這個class的<font color="red">每個測試前都會執行的區塊</font>，可以在這邊放置共用測試的資料，例如Database、JSon、List等等，這樣就不用在每個testing function裡都要重複寫一次。
2. <span class="color_yellow_FFD306">@Test</span> - 代表這是<font color="red">要被測試的function</font>，當我們呼叫JUnit runner執行class時候這個function會被執行。
3. <span class="color_yellow_FFD306">@After</span> - 在這個class的<font color="red">每個測試後都會執行的區塊</font>，用來釋放資源，例如database、server等等。
4. <span class="color_yellow_FFD306">@BeforeClass</span> - 用於static方法，<font color="red">在所有測試開始之前執行一次</font>，用於執行耗時的工作，例如：連到數據庫。
5. <span class="color_yellow_FFD306">@AfterClass</span> - 用於static方法，<font color="red">在完成所有測試後執行一次</font>，用於清理的工作，例如：與數據庫斷開連接。
6. <span class="color_yellow_FFD306">@Ignore</span> - 指定要忽略的測試。
7. <span class="color_yellow_FFD306">@Test(expected = Exception.class)</span> - 如果該方法未觸發異常，則失敗。
8. <span class="color_yellow_FFD306">@Test(timeout = 100)</span> - 如果該方法花費時間超過100毫秒，則失敗。


測試程式沒有發生錯誤、成功跑完的話，就會顯示綠色勾勾，但是如果想要有驗證行為，通常會用assert。
#### 常用assert (驗證)
1. <span class="color_yellow_FFD306">assertEquals(a, b)</span> - 是否相等。
2. <span class="color_yellow_FFD306">assertTrue(boolean)</span> - 是否為true。
3. <span class="color_yellow_FFD306">assertFalse(boolean)</span> - 是否為false。
4. <span class="color_yellow_FFD306">assertNull(a)</span> - 是否為Null。
5. <span class="color_yellow_FFD306">assertNotNull(a)</span> - 是否不是Null。
6. <span class="color_yellow_FFD306">assertThrows {}</span> - 是否拋出例外。

實際使用可參考[[JUnit範例 - 晴天9折，下雨沒折]]。