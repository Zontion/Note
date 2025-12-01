主要設定在src/test/java/com.example.app/目錄下  
新專案預設會有ExampleUnitTest這個class  
右鍵檔案>Run即可啟動(或是右上角的綠色箭頭)

JUnit主要是經由Annotation的方式來驅動測試流程。常見的三個基本Annotation

1. @Before - 在這個class的每個測試前都會執行的區塊，可以在這邊放置共用測試的資料，例如Database、JSon、List等等，這樣就不用在每個testing function裡都要重複寫一次。
2. @Test - 代表這是要被測試的function，當我們呼叫JUnit runner執行class時候這個function會被執行。
3. @After - 在這個class的每個測試後都會執行的區塊，用來釋放資源，例如database、server等等。

# 可UI測試 -

## Espresso：

參考資料：iT邦幫忙 - 從０開始，全方面自動化測試Android App ([https://ithelp.ithome.com.tw/users/20120975/ironman/2726](https://ithelp.ithome.com.tw/users/20120975/ironman/2726))