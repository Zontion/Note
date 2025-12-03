# 簡介
Java / Kotlin最常用的單元測試(Unit Test)框架，跑在JVM上，速度最快、最單純的邏輯測試工具。

# 使用方式
主要設定在src/test/java/com.example.app/目錄下
新專案預設會有ExampleUnitTest這個class
右鍵檔案>Run即可啟動(或是右上角的綠色箭頭)

JUnit主要是經由Annotation的方式來驅動測試流程

#### 常見的Annotation
1.  <span class="color_yellow_FFD306">@Before</span> - 在這個class的<font color="red">每個測試前都會執行的區塊</font>，可以在這邊放置共用測試的資料，例如Database、JSon、List等等，這樣就不用在每個testing function裡都要重複寫一次。
2. <span class="color_yellow_FFD306">@Test</span> - 代表這是<font color="red">要被測試的function</font>，當我們呼叫JUnit runner執行class時候這個function會被執行。
3. <span class="color_yellow_FFD306">@After</span> - 在這個class的<font color="red">每個測試後都會執行的區塊</font>，用來釋放資源，例如database、server等等。

#### 常用assert (驗證)
1. assertEquals(a, b) - 是否相等。
2. assertTrue(boolean) - 是否為true。
3. assertFalse(boolean) - 是否為false。
4. assertNull(a) - 是否為Null。
5. assertNotNull(a) - 是否不是Null。
6. assertThrows {} - 是否拋出例外。

實際使用可參考[[JUnit範例 - 晴天9折，下雨沒折]]。