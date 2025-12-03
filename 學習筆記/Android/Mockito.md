# 簡介
在Unit Test時，因實務上要測試的模組可以模擬Android Framework，例如創建一個假的Activity或是當你需要呼叫Context的getString()方法時

主要使用工具可google關鍵字 Mockito或MockK


# 使用方式(官方文件)
在build.gradle加入

```java
dependencies {
  // Required -- JUnit 4 framework
  testImplementation "junit:junit:$jUnitVersion"
  // Optional -- Robolectric environment
  testImplementation "androidx.test:core:$androidXTestVersion"
  // Optional -- Mockito framework
  testImplementation "org.mockito:mockito-core:$mockitoVersion"
  // Optional -- mockito-kotlin
  testImplementation "org.mockito.kotlin:mockito-kotlin:$mockitoKotlinVersion"
  // Optional -- Mockk framework
  testImplementation "io.mockk:mockk:$mockkVersion"
}
```
範例參考：
```kotlin
import android.content.Context
import org.junit.Assert.assertEquals
import org.junit.Test
import org.junit.runner.RunWith
import org.mockito.Mock
import org.mockito.junit.MockitoJUnitRunner
import org.mockito.kotlin.doReturn
import org.mockito.kotlin.mock

private const val FAKE_STRING = "HELLO WORLD"

@RunWith(MockitoJUnitRunner::class)
class MockedContextTest {

  @Mock
  private lateinit var mockContext: Context

  @Test
  fun readStringFromContext_LocalizedString() {
    // Given a mocked Context injected into the object under test...
    val mockContext = mock<Context> {
        on { getString(R.string.name_label) } doReturn FAKE_STRING
    }

    val myObjectUnderTest = ClassUnderTest(mockContext)

    // ...when the string is returned from the object under test...
    val result: String = myObjectUnderTest.getName()

    // ...then the result should be the expected one.
    assertEquals(result, FAKE_STRING)
  }
}
```
# 其他注意
Mockito不支援static與singleton，如有需求需額外處理