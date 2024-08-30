# Gson

## 使い方

### 1.build.gradleのimplementationに依存関係追加

```kotlin
implementation("com.google.code.gson:gson:2.10.1")
```

### 2.解析するJsonファイルの形式に合わせたデータクラスを作成

例：

```json
[
  {
    "eventId": "EVENT_0000",
    "eventTitle": "WorldSkills Competition 2022 Special Edition",
    "eventText": "This year, 61 international skill competitions will take place across Europe, North America, and East Asia from September to November 2022.",
    "eventReadStatus": false,
    "eventPictures": "events_00_A.jpg"
  },
  {
    "eventId": "EVENT_0001",
    "eventTitle": "A unique format for international skill competitions",
    "eventText": "WorldSkills is preparing a unique format for international skill competitions in 2022, showcasing 61 skills in 15 different countries and regions around the world. WorldSkills Competition 2022 Special Edition (WSC2022SE) is the official replacement for WorldSkills Shanghai 2022, cancelled in May due to the pandemic.",
    "eventReadStatus": false,
    "eventPictures": "events_01_A.jpg"
  },...
```

このような形式のデータが続く場合、

・DataClassは、

```kotlin
//jsonのキーの名前と変数名を合わせる必要がある
data class EventData(val eventId: String,val eventTitle: String,val eventText: String,var eventReadStatus: Boolean,val eventPictures: String)
```

### 3.データ取得

・データを格納するリスト変数を作成する

```kotlin
  private var dataList = mutableListOf<EventData>()
```

・Gsonを呼び出しておく

```kotlin
    private lateinit var gson: Gson
//~省略~
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        gson = Gson()
```

・データを取得する

```kotlin
private fun getData(file: File){
        val text = file.bufferedReader().use { it.readText() }
        //変換する形式を指定
        val type = object: TypeToken<List<EventData>>(){}.type
        dataList = gson.fromJson(text,type)
    }
```

### jsonファイルの書き換え

・格納されているリスト変数の値を書き換えて、それをjsonファイルに上書き

```kotlin
    private fun changeData(file: File){
        dataList[0].eventReadStatus = true //データクラスでの変数定義の時にvarで定義しておく
        val newJson = gson.toJson(dataList)
        file.writeText(newJson)
        getData(file)
    }
```

### jsonファイルからデータの削除

・リストからデータを削除して、そのリスト変数の中身で上書きする

```kotlin
private fun deleteData(file: File){
        dataList.removeAt(0)
        val newJson = gson.toJson(dataList)
        file.writeText(newJson)
        getData(file)
    }
```

### 元のキー名にスペースなどが含まれている場合

・そのまま変数名とすることができないので、`@SerializedName`アノテーションを使用

・例: `event Id`というキーがある場合

```kotlin
data class Sample(@SerializedName("event Id") var eventId: Int = 0)
```

### キーの値がバラバラ（直接バリューのようになっている）時

・`Map<String, Any>`で取得する (Anyの部分はデータ形式に合わせる)

例：`Map<String,List<ChildItem>>`　など


