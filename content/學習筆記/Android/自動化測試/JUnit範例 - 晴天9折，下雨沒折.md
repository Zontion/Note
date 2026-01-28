
本篇學習關鍵字：
* Stub：假物件，模擬外部相依物件的回傳結果。
* Mock：假物件，用來驗證目標與相依物件的互動。
* Test：測試程式。
* SUT：System under test，被測試物件(也可以是方法)。

### 在進入正題前，先認識[[假物件Stub與Mock]]
___

以下為正文
### 開發情境：「晴天9折，下雨沒折」

新增一個Umbrella類別，內容有方法totalPrice()，帶入數量與價格後，依照內容打isSunny的API取得是否晴天，若晴天則9折

```kotlin
fun main() {
	val umbrella = Umbrella()
	val totalPrice = umbrella.totalPrice(1, 600)
	println("totalPrice:$totalPrice")
}
	
class Umbrella {
	//雨傘價格計算
	fun totalPrice(quantity: Int, price: Int): Int {
		val isSunny = Weather().isSunny() //取得是否是晴天的API
		//數量乘以價錢
		var price = quantity * price 
		if (isSunny) {
			//晴天9折
			price = (price * 0.9).toInt()
		}
		
		return price
	}
}

class Weather {
	//API範例
	fun isSunny(): Boolean {
		val result = ...
		return result
	}
}
```
但對totalPrice而言，取得是否晴天的API為外部相依物件，無法控制其回傳值，因此無法做測試。因此為了解決這個問題，可以<font color="red">把取得天氣API改為透過interface</font>，並把totalPrice方法參數增加需帶入此interface，原本打API的Weather更改寫法。如下

```kotlin
class Umbrella {
	//雨傘價格計算
	fun totalPrice(weather: IWeather, quantity: Int, price: Int): Int {
		val isSunny = weather.isSunny() //取得是否是晴天的API
		//數量乘以價錢
		var price = quantity * price 
		if (isSunny) {
			//晴天9折
			price = (price * 0.9).toInt()
		}
		
		return price
	}
}

class Weather: IWeather {
	override fun isSunny(): Boolean {
		val result = ...
		return result
	}
}

interface IWeather {
	fun isSunny(): Boolean
}
```
此改動最主要的原因就是讓是否晴天的控件改為由外部傳入，這種做法叫做**依賴注入**

後續就可以建立假的天氣類別來做測試，如下
```kotlin
class StubWeather: IWeather {
	var fakeIsSunny = false
	
	override fun isSunny(): Boolean {
		return fakeIsSunny
	}
}
```
JUnit即可寫
```kotlin
@Test
fun totalPrice_sunnyDay() {  
	val umbrella = Umbrella()
	val weather = StubWeather() //建立假天氣
	weather.fakeIsSunny = true //設定晴天
	
	val actual = umbrella.totalPrice(weather, 3, 100)
	val expected = 270
	Assert.assertEquals(expected, actual)
}
```

以上假物件的部分，由外部傳入，用以驗證回傳結果，故稱為Stub。
# Stub

Stub定義如下圖

![[img_stub_1.png]]

>[!note] 名稱介紹
>* Test：測試程式，UmbrellaTest.totalPrice_sunnyDay()  
>* SUT：被測試物件，也就是Umebralla.totalPrice()用來計價的方法。  
>* Stub：假天氣物件StubWeather，用來回傳預期是晴天或雨天。

![[img_stub_2.png]]

# Mock

Mock定義如下圖

![[img_mock_1.png]]

假設今天雨傘網路下單完成後要寄送mail給使用者
```kotlin
class Order {
	fun insertOrder(email: String, quantity: Int, price: Int) {
		val weather = Weather()
		val umbrella = Umbrella()
		umbrella.totalPrice(weather, quantity, price) //前面尚未使用Stub的寫法
		
		val emailUtil = EmailUtil()
		emailUtil.sendCustomer(email)
	}
}
```

其中EmailUtil是一個外部相依物件，所以先一樣調整為**依賴注入**，並且新增參數IEmailUtil

```kotlin
interface IEmailUtil {
	fun sendCustomer(email: String)
}

class EmailUtil: IEmailUtil {
	override fun sendCustomer(email: String) {
		//發送Email
	}
}

fun insertOrder(email: String, quantity: Int, price: Int, eamilUtil: IEmailUtil) {
	val weather = Weather()
	val umbrella = Umbrella()
	umbrella.totalPrice(weather, quantity, price) //前面尚未使用Stub的寫法
	
	emailUtil.sendCustomer(email) //發送Email
}
```

開始建立Mock模擬物件

```kotlin
class MockEmailUtil: IEmailUtil {
	var receiveEmail: String? = null // 用來記錄sendCustomer傳進來的Email
	override fun sendCustomer(email: String) {
		receiveEmail = email
	}
}
```

最後透過驗證Mock物件的receiveEmail是否與參數的Email相符
```kotlin
@Test
fun testInsertOrder() {
	val order = Order()
	val mockEmailUtil = MockEmailUtil()
	
	val userEmail = "abcdefg@gmail.com"
	order.insertOrder(userEmail, 1, 200, mockEmailUtil)
	
	Assert.assertEquals(userEmail, mockEmailUtil.receiveEmail)
}
```

相關關鍵字對應如下圖
![[img_mock_2.png]]

# 總結
雖然已經了解Stub與Mock，測試時要建立Stub相對簡單，但如果每次都要建立Mock並做出不同的操作行為則會非常麻煩，此時就可以考慮使用[[Mock Anrdoid Framework]]來更方便地做到模擬物件行為。
