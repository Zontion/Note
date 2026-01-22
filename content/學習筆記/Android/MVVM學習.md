>[!abstract]
>1. 本次學習會用到 Java + LiveData + Retrofit + Gson
>2. 相關工具後面都會介紹
>3. 無使用Databinding，因為私心不喜歡

## 基本介紹
### **MVVM是Model-View-ViewModel**的簡稱，三者扮演的角色為：

**View**：Activity、Fragment或custom view，本身不做邏輯處理，當使用者跟UI有互動時將指令傳給ViewModel處理，透過其獲得所需的資料並顯示。

**ViewModel**：接收View的指令並對Model請求資料，將取得的資料保存起來供View使用。

**Model**：管理所有的資料來源，例如API、資料庫和SharedPreference，當ViewModel來請求資料時從正確的來源取得資料並回傳。

>[!info]
>ViewModel是屬於Android Jetpack裡的lifecycle類，可以有效的解決記憶體洩漏，及難以處理的Activity生命週期問題。以往我們會把取得的資料存在Activity裡，用來應付各種情況所需。但當你的螢幕旋轉畫面上，會發現上面的資料不見了。這是因為旋轉時Activity會先被銷毀(destoryed)再重新產生(onCreated)。所以之前放在Activity裡的資料就會因為這樣而不見了。
>
>另外，即使你的App不讓使用者旋轉，Activities、Fragments 和 views 還是可能在任何時候被destroyed。例如當你開啟App後，離開到別的App，隔了一天再回來app時。你的Activity可能已經被回收了，這時候你開App即會重新產生一個Activity，這時資料就會不見。或是你的app可能會閃退。這種錯誤通常不容易測試。因為你不會把App放一天再開起來測試。但對使用者來說，這可能是經常會發生的錯誤。
>
>所以我們需要另一個比Activity生命週期更長的地方來存放資料，ViewModel正可以為我們解決這個問題。

## MVVM架構圖
![[img_mvvm.png]]]
撰寫重點：原則上就是Model層不會知道View與ViewModel層，ViewModel層不會知道View層。

---

## 實作1：透過ViewModel取得字串資料顯示在UI
>前置作業：建立新專案，可新增一個folder -> view，將MainActivity移至裡面(看起來更有MVVM分類的感覺XD)，layout設置一個TextView、一個Button、一個ProgressBar。

新增一個folder -> viewmodel，裡頭新增一個MainViewModel
```java
public class MainViewModel extends ViewModel {
    public String getString() {
        return "123";
    }
}
```
在MainActivity宣告
```java
private MainViewModel viewModel;
```
在onCreate()裡初始化
```java
viewModel = new ViewModelProvider(this).get(MainViewModel.class);
```
在Button click事件中設定
```java
textView.setText(viewModel.getString());
```
TextView即顯示"123"

---

## LiveData
LiveData 是一個用於持有數據並可以**監聽數據變動**的元件。

**LiveData是一個觀察者模式**，比起一般的observable類別，**LiveData具有生命週期感知的功能**。，因此觀察者可以指定某一個具有生命週期的元件給 LiveData，例如 Activity, Fragment，並在生命週期是"活躍"的時候進行更新，也就是生命週期裡的 onStart 和 onResume，也因為此特性，**所以通常搭配 ViewModel 使用**。

---

## LiveData優點
1. **UI 與數據保持一致**：觀察者模式最基本的功能。
2. **節省資源**：當 Activity 不在活躍時期時是不會收到 LiveData 的任何事件的，能讓系統省去不必要的接受事件。
3. **避免內存泄露**：因為 LiveData 能夠感知生命週期，所以當 Activity 被銷毀時，LiveData 也會隨之銷毀，避免不必要的引用而造成的內存泄露。
4. **更好的使用者體驗**：如果今天有一個功能是：數據更新完成後跳出 Toast 告訴使用者更新完成，但如果這時候應用是處在後台，也就是非活躍狀態，突然跳出一個 Toast，反而會讓使用者覺得奇怪。若是使用 LiveData 則會讓應用處在 活躍狀態時 才跳 Toast，能帶來更好的使用者體驗。

---

## 實作2：打API成功回傳後顯示資料在UI上
>測試網站 <https://jsonplaceholder.typicode.com/>
>GET網址 <https://jsonplaceholder.typicode.com/albums/1>
>POST網址 <https://jsonplaceholder.typicode.com/albums>

新增網路使用權限
```xml
<uses-permission android:name="android.permission.INTERNET"/>
```
引入okhttp套件
```groovy
implementation 'com.squareup.okhttp3:okhttp:4.7.2'
```
新增一個folder -> model，裡頭新增一個AlbumsRepository，加上一個callApi方法與一個ApiCallBack interface
```java
public class AlbumsRepository {

    public void callApi(final OnApiCallBack onApiCallBack) {
        OkHttpClient client = new OkHttpClient().newBuilder().build();
        Request request = new Request.Builder()
                .url("https://jsonplaceholder.typicode.com/albums/1")
                .build();

        Call call = client.newCall(request);
        call.enqueue(new Callback() {
            @Override
            public void onFailure(@NotNull Call call, @NotNull IOException e) {
                onApiCallBack.onResult("Error");
            }

            @Override
            public void onResponse(@NotNull Call call, @NotNull Response response) throws IOException {
                onApiCallBack.onResult(response.body().string());
            }
        });
    }

    public interface OnApiCallBack {
        void onResult(String result);
    }
}
```
回到MainViewModel宣告
```java
private AlbumsRepository repository = new AlbumsRepository();
```
並新增方法
```java
public void callAlbumsApi() {
    repository.callApi(new AlbumsRepository.OnApiCallBack() {
        @Override
        public void onResult(String result) {
        }
    });
}
```
並且宣告LiveData物件
```java
private MutableLiveData<String> liveData;
```
與Singleton
```java
public MutableLiveData<String> getLiveData() {
    if (liveData == null) {
        liveData = new MutableLiveData<String>();
    }
    return liveData;
}
```
回到Activity新增
```java
viewModel.getLiveData().observe(this, new Observer<String>() {
    public void onChanged(String s) {
        progressBar.setVisibility(View.INVISIBLE);
        textView.setText(s);
    }
});
```
再把按鈕事件調整為呼叫callApi
```java
viewModel.callAlbumsApi();
```
此時即可測試當UI顯示資料後，翻轉手機是否依舊有顯示資料。

---
## Retrofit
Retrofit是專為**API連線而生的第三方套件**，與API連線的效率非常高，最特別的是其規範的REST框架讓程式高度解耦，好寫易維護，被譽為API連線的教科書。另外它跟**OkHttp同為Square公司出品，兩者可以完美整合發揮更多功能**。

## Gson
Gson是google釋出的library，主要為了方便將Java物件序列化**Serialization**至輕量化的封包格式JSON，提供了很多方便快捷的方法(其他Json解析庫還有: Jackson、FastJson…等較不常用)。
```
Serialization：指將資料結構或物件狀態轉換成可取用格式（例如存成檔案，存於緩衝，或經由網路中傳送)
```

---

## 實作3：導入Retrofit
引入retrofit與Gson套件
```groovy
implementation 'com.squareup.retrofit2:retrofit:2.2.0'
implementation 'com.squareup.retrofit2:converter-gson:2.2.0'
```
新增一個folder -> data，裡面新增Data Class Albums並且設定好constructor & getter
```java
public class Albums {

    private int userId;
    private int id;
    private String title;

    public Albums(int userId, int id, String title) {
        this.userId = userId;
        this.id = id;
        this.title = title;
    }

    public int getUserId() {
        return userId;
    }

    public int getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }
}
```
新增一個folder -> api，裡面新增public interface AlbumsService
```java
public interface AlbumsService {
    
    @GET("albums/1")    // 設置一個GET連線，路徑為albums/1
    Call<Albums> getAlbums();    // 取得的回傳資料用Albums物件接收

    @GET("albums/{id}") // 用{}表示路徑參數，@Path會將參數帶入至該位置
    Call<Albums> getAlbumsById(@Path("id") int id);

    @POST("albums") // 用@Body表示要傳送Body資料
    Call<Albums> postAlbums(@Body Albums albums);
}
```

>[!info]
>Retrofit的create只吃interface參數

新增一個folder -> retrofit，裡面新增RetrofitManager
```java
public class RetrofitManager {

    // 以Singleton模式建立
    private static RetrofitManager mInstance = new RetrofitManager();

    private AlbumsService albumsService;

    private RetrofitManager() {

        // 設置baseUrl即要連的網站，addConverterFactory用Gson作為資料處理Converter
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://jsonplaceholder.typicode.com/")
                .addConverterFactory(GsonConverterFactory.create())
                .build();

        albumsService = retrofit.create(AlbumsService.class);
    }

    public static RetrofitManager getInstance() {
        return mInstance;
    }

    public AlbumsService getAPI() {
        return albumsService;
    }
}
```
這邊url調整為沒有帶參數的短網址
addConverterFactory用Gson作為資料處理Converter
一般情況會把baseUrl抽出 提供給子類別去塞
Service的部分也是 不會直接寫在這邊

常見的 converter 有以下幾種：
Gson: com.squareup.retrofit2:converter-gson
Moshi: com.squareup.retrofit2:converter-moshi
Protobuf: com.squareup.retrofit2:converter-protobuf
也可以在以下這個網址看是否有你需要的 converter。 <https://mvnrepository.com/artifact/com.squareup.retrofit2>


回到AlbumsRepository宣告
```java
// 1. 宣告MyAPIService
private AlbumsService service;
```

把callApi方法內容改為
```java
public void getAlbums(final OnAPIFinish onAPIFinish) {
    // 2. 透過RetrofitManager取得連線基底
    albumsService = RetrofitManager.getInstance().getAPI();

    // 3. 建立連線的Call，此處設置call為myAPIService中的getAlbums()連線
    Call<Albums> call = albumsService.getAlbums();
    // 4. 執行call
    call.enqueue(new Callback<Albums>(){
        @Override
        public void onResponse(Call<Albums> call, Response<Albums> response) {
            // 連線成功
            // 回傳的資料已轉成Albums物件，可直接用get方法取得特定欄位
            String title = response.body().getTitle();
            Log.d("title", title);
            onAPIFinish.onResult(response.body());
        }

        @Override
        public void onFailure(Call<Albums> call, Throwable t) {
            // 連線失敗
        }
    });
}
```
因為回傳格式已經改為Albums所以把原本String的地方都調整完即可
