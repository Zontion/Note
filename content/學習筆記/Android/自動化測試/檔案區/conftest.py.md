```python
"""
Pytest 共用設定：Appium WebDriver 的建立與銷毀。
此範例為測試已經安裝在模擬器上的 ExampleApp
"""
import pytest
from appium import webdriver
from appium.options.android import UiAutomator2Options

# 已安裝的 Example app：用 appPackage + appActivity 啟動

APPIUM_SERVER = "http://127.0.0.1:4723"

def _build_options():
	opts = UiAutomator2Options()
	opts.platform_name = "Android"
	opts.automation_name = "UiAutomator2"
	opts.device_name = "emulator-5554" # 若裝置名稱不同請改 adb devices 看到的
	opts.app_package = "com.example.app"
	opts.app_activity = "com.example.startup.StartupActivity"
	return opts

@pytest.fixture(scope="function")
def driver():
	drv = webdriver.Remote(APPIUM_SERVER, options=_build_options())
	yield drv
	drv.quit()
```

>[!info] （註1）conftest.py檔案內容介紹
>* **APPIUM_SERVER**：Appium 位址，預設 http://127.0.0.1:4723。
>* **_build_options()**：組出 UiAutomator2Options，設定：
>	* 平台與自動化：platform_name、automation_name（UiAutomator2）。
>	* 裝置：device_name（例：emulator-5554，要與 adb devices 一致。
>	* 已安裝的 app：app_package（com.example.app）、app_activity（StartupActivity）
>* **driver fixture**：每個 test 執行前立 webdriver.Remote(...)，執行完後 quit()，測試函式只要參數寫 driver 就會收到這個實例。