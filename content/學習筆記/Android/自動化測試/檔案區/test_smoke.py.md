```python
"""
簡單的 smoke test：啟動 app 並檢查是否在畫面上。
請先啟動 Appium Server（終端執行 appium），並接上裝置或模擬器。
"""
import pytest
from appium.webdriver.common.appiumby import AppiumBy
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

@pytest.mark.skip(reason="需先啟動 Appium、裝置與 APK，再移除此行執行")
def test_app_launches(driver):
	"""確認 app 可啟動（預設等待 10 秒內出現 package 對應的 activity）。"""
	wait = WebDriverWait(driver, 10)
	# 以目前 activity 或任一可見元素做簡單驗證，可依實際 UI 改定位方式
	activity = driver.current_activity
	assert activity is not None
	# 若有固定元素（例如 resource-id 或 content-desc），可改為：
	# el = wait.until(EC.presence_of_element_located((AppiumBy.ID, "某 resource-id")))
	# assert el.is_displayed()
```

>[!info] （註2）test_smoke.py檔案內容介紹
>* 一個簡單的 smoke test，確認「app 能被 Appium 啟動且沒有當掉」。
>	* **test_app_launches(driver)**：依賴 conftest 的 driver，不自己建連線。
>	* **邏輯**：取得 driver.current_activity，用 assert activity is not None 確認有進入某個 activity，代表 app 有成功啟動。
>	* **註解**：示範之後若要改成「等某個 UI 元素出現」可用 WebDriverWait + AppiumBy.ID 等寫法。
