# Stub (單純提供固定回傳值)

情境：一個Repository需要API回傳資料才能跑，但你不想用真的API。

```kotlin
interface UserApi {
	fun getUser(id: Int): String
}

//寫一個假的API 只回傳固定值
class UserApiStub: UserApi {
	override fun getUser(id: Int): String {
		reutnr "StubUser"
	}
}
```
JUnit測試如以下：
```kotlin
@Test
fun testWithStub() {
	val api = UseApiStub()
	val result = api.getUser(123)
	
	Assert.assertEquals("StubUser", result) //只驗證回傳資料
}
```
>**Stub特性**：
>* Stub不會紀錄是否被呼叫
>* 不會檢查「被呼叫的參數(範例中的123)為何」
>* 單純提供固定值讓測試能跑

# Mock (可驗證呼叫行為)

同樣情境，但是當今天需要驗證API是否有被呼叫且呼叫的參數為何就需要使用Mock
```kotlin
class UserApiMock: UserApi {
	var called = false
	var calledWithId = id
	
	override fun getUser(id: Int): String {
		called = true //紀錄是否被呼叫
		calledWithId = id //紀錄呼叫時的參數
		return "MockUser"
	}
}
```
JUnit測試如以下：
```Kotlin
@Test
fun testUsingMock() {
	val api = UserApiMock()
	api.getUser(123)
	
	Assert.assertTrue(api.called)
	Assert.assertEquals(123, api.calledWithId)
}
```

>**Mock特性：**
>* Mock可以記錄是否被呼叫
>* 可以驗證呼叫行為
>* 可以知道呼叫幾次、參數是什麼

| 功能                          | Stub | Mock |
| :-------------------------- | :--: | :--: |
| 回傳假資料                       |  O   |  O   |
| 不跑真API                      |  O   |  O   |
| 可用在ViewModel / Repository測試 |  O   |  O   |
| 紀錄是否被呼叫                     |  X   |  O   |
| 紀錄呼叫參數                      |  X   |  O   |
| 驗證呼叫行爲                      |  X   |  O   |
# 總結

### Stub：一個可控制的假物件，用以取代原本相依的第三方元件，可以使單元測試不用去擔心第三方的邏輯會導致測試失敗。

### Mock：一個假物件，可以用來驗證本身的邏輯與第三方元件的互動方式是否正確。

><font color="red">Mock可能會導致測試失敗，Stub僅僅用以取代真實物件，並不會導致測試失敗。</font>
