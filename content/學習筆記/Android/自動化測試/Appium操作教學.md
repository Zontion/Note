操作相關紀錄可參考：[[Appium操作紀錄]]

#### 操作步驟紀錄(以Mac且已安裝Android Studio為例)：
1. **安裝Appium：**
	>terminal下指令`npm install -g appium` (遇到權限問題 可在最前面加上`sudo`並輸入使用者密碼 可用`appium --version`檢查是否成功)
2. **將Appium測試工具設定為UIAutomator2：**
	>terminal下指令`appium driver install uiautomator2`(可用`appium driver list --installed`檢查是否成功)
3. **在專案底下加入自動化package(ex: app/automation)以下檔案：**
>1. [[conftest.py]] - **可選**，Pytest 的共用設定，主要負責「建立與關閉 Appium driver」，所有測試都可透過 fixture 使用同一個 driver。（[註1](#^note1)）
>2. [[requirements.txt]] - **可選**，列出Python的依賴，後續用 pip install -r requirements.txt 就可安裝，專案有寫這個檔案的話，別人（或 CI）相同指令就能還原出跟你一樣的環境。
>3. [[test_smoke.py]] - test_xxx，測試案例。
4. **創建venv環境：**
>使用指令`cd /Users/{user_name}/{project_name}/{automation_path}`輸入`python -m venv venv`（如不行 可試試python3 -m...），此步驟是為了在此路徑創建venv環境（註1），裡面有獨立的Python解譯器與`pip`，以及之後用`pip install`裝的套件都會放在此資料夾，不會影響到你系統的Python。
5. **安裝套件：**
>下指令`pip install -r requirements.txt`
6. **啟動Appium Server：**
>開啟一個新的terminal視窗輸入`appium`啟動一個appium server。
7. ** 開啟另一個terminal視窗使用指令：**
>`cd /Users/{user_name}/{project_name}/{automation_path}`然後輸入`source venv/bin/activate`啟動虛擬環境(venv)。
8. **執行檔案：**
>最後輸入`pytest test_smoke.py -v`執行test_smoke.py這個檔案內容。（遇到$ANDROID_HOME看[註2](#^note2)）

>[!warning] 注意，當測試流程包含對WebView內容做操作時
>需要做以下設定：
>1. App要設定`WebView.setWebContentsDebuggingEnabled(true)`
>2. 啟動Appium Server不要使用`appium`，改用`appium --allow-insecure=uiautomator2:chromedriver_autodownload`

>[!example] 當未來重新開啟時（重開機後），僅需做以下操作：
>1. 開啟terminal輸入`appium`啟動伺服器。
>2. 開啟另一個terminal cd到`/Users/{user_name}/{project_name}/{automation_path}`路徑輸入`source venv/bin/activate`啟動venv環境。
>3. 輸入`pytest {file_name}.py -v`執行該file。

> [!info] 註1 ^note1
> **什麼是venv?**
> venv 是 Python 內建的「虛擬環境」工具，用來在某個專案底下建立一個獨立的 Python 環境。
> 
> **為什麼要用?**
> 不同專案可能需要不同版本的套件（例如 A 專案要 pytest 6，B 專案要 pytest 7），若都裝在「全機同一個 Python」裡會互相衝突。用 venv 可以讓每個專案有自己的 site-packages，彼此不影響。

>[!info] 註2 ^note2
> **遇到$ANDROID_HOME環境變數問題**
>在啟動appium的terminal視窗（即有輸入`appium`指令的那個，若是啟動中可以先ctrl+c關閉）依序輸入
>1. `echo 'export ANDROID_HOME=$HOME/Library/Android/sdk' >> ~/.zshrc`
>2. `echo 'export PATH=$ANDROID_HOME/platform-tools:$PATH' >> ~/.zshrc`
>3. `source ~/.zshrc`
>再輸入`echo $ANDROID_HOME`測試是否有成功印出路徑。
>最後輸入`appium`重啟即可。
