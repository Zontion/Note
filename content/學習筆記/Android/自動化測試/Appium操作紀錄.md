### 學前須知：(測試)
 Appium測試App有兩種方式：
1. 測試裝置中已安裝的Apk。
2. 同時Run Apk與測試。

本次教學是以1為主。

### 測試技巧：
- 測試時可寫tag，並且在執行pytest指令時，可以設定變數當作是否開啟測試MODE的前綴，例：`APROJECT_DEBUG=1 pytest test_login.py -v -s，以APROJECT_DEBUG為1時，後續appium測試狀況print出相關步驟log，當失敗時方便抓取問題原因。

### 當流程有WebView時：

1. 當操作流程有WebView時，需設定`WebView.setWebContentsDebuggingEnabled(true)`以及啟動Appium使用`appium --allow-insecure=uiautomator2:chromedriver_autodownload`。
2. 