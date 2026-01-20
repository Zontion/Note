# 簡介

當你的程式與Android framework相依，例如Context，在測試時就會是Instrumented tests，需要在實體裝置或模擬器執行。這會讓你的測試變慢，透過Mock Android framework，讓你仍是使用Local unit tests。

### Mock框架常見的有以下兩種：

1. **Mockito**：Java 社群主流的 mocking library，用來模擬（mock）物件行為。官方 Android / Java 支持，Kotlin 也能用，但語法對 Kotlin 某些特性（如 final class、data class）不太友好，需要額外配置。
2. **MockK**：Kotlin 原生的 mocking library，完全支援 Kotlin 特性（final class、extension function、coroutine 等），語法更 Kotlin 化。

>[!tip]
>若專案以Android+Kotlin開發，更推薦使用<font color="red">**Mockito更偏向Java、MockK更偏向Kotlin**</font>。

___
# Mockito

### 使用方式
在build.gradle加入
```java
dependencies {
	testImplementation 'org.mockito:mockito-core:$mockitoVersion'
	testImplementation 'org.mockito:mockito-inline:$mockitoVersion'
}
```
mockito-core是基礎版本，mockito-inline為mockito-core的擴充，mockito-inline使用Byte Buddy作為Mock Maker，可支援更多的使用情境，如：static、final及建構子等。

>[!info]
>Byte Buddy - 用來動態生成或修改Java類別bytecode的工具。Byte Buddy會生成一個新的class。所以可以做到改寫getter行為，因此可以做到回傳mock值。

以[[JUnit範例 - 晴天9折，下雨沒折]]中，單純用JUnit要使用Stub物件時，需要先寫一個StubWeather去繼承IWeather並實作isSunny()回傳假資料。

若改用Mocktio則只需要在@Test裡面寫：
```kotlin
@Test  
fun test_totalPriceByMockito_sunnyDay() {  
    val umbrella = Umbrella()  
  
    val weather = mock(IWeather::class.java)  
    `when`(weather.isSunny()).thenReturn(true)  
  
    val actual = umbrella.totalPrice(weather, 3, 100)  
    val expected = 270  
    Assert.assertEquals(expected, actual)  
}
```
>[!tip]
>因when在Kotlin為保留字，因此需要加上\`\`，變成\`when\`

### Mockito限制

Mockito在舊版本不支援`private`、`final`、`static`、`singleton`等。(Mockito 3好像開始有支援，要使用可以確認版本有沒有支援，沒有的話可以在使用其他工具協助，如：PowerMock）

___

# MockK

MockK是一個專門為Kotlin創建Mocking的框架，所有 Mockito 可以做到的事情 Mockk 都可達成，並且克服了 Mockito 在 Kotlin 中的限制。

### 使用方式
在build.gradle加入
```kotlin
dependencies {
	testImplementation "io.mockk:mockk:$mokkVersion"
}
```

以[[JUnit範例 - 晴天9折，下雨沒折]]中，單純用JUnit要使用Stub物件時，需要先寫一個StubWeather去繼承IWeather並實作isSunny()回傳假資料。

若改用MockK則只需要在@Test裡面寫：

```kotlin
@Test  
fun test_totalPriceByMockK_sunnyDay() {  
    val umbrella = Umbrella()  
  
    val weather = mockk<IWeather>()  
    every { weather.isSunny() } returns true  
  
    val actual = umbrella.totalPrice(weather, 3, 100)  
    val expected = 270  
    Assert.assertEquals(expected, actual)  
}
```



資料參考：
Mockito & MockK框架 - https://hackmd.io/@AlienHackMd/S1aid2wao#Mockito-%E5%9C%A8-Kotlin-%E4%B8%AD%E7%9A%84%E9%99%90%E5%88%B6