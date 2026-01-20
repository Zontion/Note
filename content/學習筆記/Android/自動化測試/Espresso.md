# 簡介

### 使用方式
在build.gradle加入
```java
dependencies {
	androidTestImplementation 'androidx.test.ext:junit:1.1.5'  
	androidTestImplementation 'com.android.support.test:rules:1.0.2'  
	androidTestImplementation 'com.android.support.test:runner:1.0.2'  
	androidTestImplementation 'androidx.test.espresso:espresso-core:3.7.0'  
	androidTestImplementation 'androidx.test.espresso:espresso-contrib:3.7.0' //如果要測試RecyclerView等組件，需添加這個
}
```
另外在android.defaultConfig加上
```java
android {
	...
	defaultConfig {
		...
		testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
	}
}
```

UI Test主要要做的就是三件事：
1. 找出View
2. 操作View
3. 比對View

針對上述三件事，列出Espresso的對應方法
1. 找出View - 使用<font color="red">ViewMatcher</font>，方法有`withId`與`withText`......等。
2. 操作View - 使用<font color="red">ViewInteratction</font>，先使用`onView`建立，然後可以呼叫`perform`來進行`click`、`typeText`等操作。
3. 比對View - 使用<font color="red">ViewAssertion</font>，先使用`matches`建立，然後再`ViewInteratction`下使用`check`來連結`ViewAssertion`。
   ```。

範例：
一個`account`型態為TextView，一個`submit`型態為Button
```Kotlin
submit.isEnabled = false 
account.addTextChangedListener(object : TextWatcher { 
	override fun afterTextChanged(s: Editable?) { 
	} 
	
	override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
	} 
	
	override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
		if (s != null) { 
			submit.isEnabled = s.isNotEmpty() 
		} else { 
			submit.isEnabled = false 
		} 
	} 
})
```
說明：當`account`有輸入文字就把`submit`設為可點擊。

寫測試時，可以檢查當`account`有輸入時，`submit`的狀態有沒有跟著改變。如下：
```Kotlin
@RunWith(AndroidJUnit4::class)
class LoginActivityTest {
	@Rule 
	@JvmField 
	val activityRule = ActivityTestRule(LoginActivity::class.java) //要測試的Activity
	
	@Test 
	fun checkSubmitState() {
		onView(withId(R.id.submit)).check(matches(not(isEnabled()))) //檢查初始狀態isEnable是否為false
		onView(withId(R.id.account)).perform(typeText("ABC")) //輸入文字
		onView(withId(R.id.submit)).check(matches(isEnabled())) //檢查狀態是否改為isEnable true
		onView(withId(R.id.account)).perform(clearText()) //清除文字
		onView(withId(R.id.submit)).check(matches(not(isEnabled()))) //檢查狀態是否改回isEnable false
	}
}
```

`onView`類似起手式，`withId`則幫助我們找到需要的`View`，再來就由`perform`來執行動作，最後由`check`來執行檢查，另外有要做反向判斷可以使用`not`來執行。

常見驗證方法：
* isEnable
* isDisplayed - 是否顯示(存在)。
* text is：檢查文字是否為該文字。

#### Espresso優點
1. Google 官方推薦的 UI 測試框架（由官方維護，持續更新，穩定性高）
2. 與 Android Studio 深度整合（可直接在 IDE 中執行和除錯測試）
3. 執行速度快，適合單一App內的UI測試（與App在同一個process，相較於其他UI測試框架，執行效率較高）
4. API 簡潔易用，學習曲線平緩（語法直觀，容易上手）
5. 自動等待 UI 操作完成，減少 flaky tests（內建同步機制，避免時序問題）
6. 支援Data Binding和RecyclerView測試（針對Android常用元件提供專門的測試API）
