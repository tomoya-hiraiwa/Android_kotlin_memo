# XML

## 名前空間(NameSpace)について

```xml
<<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
```

のように`xmlns`という属性を用いて作成する

・あるデータごとのまとまりを作るもの（メソッドに対するクラスのような役割）

例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root xmlns:person="http://example.com/person"
      xmlns:book="http://example.com/book">
  <person:name>John Doe</person:name>
  <book:title>The Great Gatsby</book:title>
</root>
```

この場合、`person`,`name`属性は`http://example.com/person`に属し、

`book`,`title`属性は`http://example.com/book`に属する


## XmlPullParserによるデータの取得

例

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        CoroutineScope(Dispatchers.IO).launch {
            val testList = mutableListOf<Test>()
            val xmlParser = applicationContext.resources.getXml(R.xml.test_file)
            while (xmlParser.eventType != XmlPullParser.END_DOCUMENT) {
                if (xmlParser.eventType == XmlPullParser.START_TAG && xmlParser.name == "data") {
                    val name = xmlParser.getAttributeValue(null, "name")
                    val count = xmlParser.getAttributeValue(null, "count")
                    testList.add(Test(name, count))
                }
                xmlParser.next()
            }
            xmlParser.close()
            println(testList)
        }
    }
}
data class Test(val name: String,val count: String)
```

・res/xml/にファイルを配置する

### Parser基本コード

```kotlin
  fun xmlParser(context: Context): Any {
        val factory = XmlPullParserFactory.newInstance()
        val parser = factory.newPullParser()
        parser.setInput(context.assets.open("enter file name"), "UTF-8")
        //enter some variable in this
        var eventType = parser.eventType
        while (eventType != XmlPullParser.END_DOCUMENT) {
            if (eventType == XmlPullParser.START_TAG) {
                when (parser.name) {
                    //write code that how to get data
                }
            }
            else if (eventType == XmlPullParser.TEXT){
                //write code that how to get data
            }
            eventType = parser.next()
        }
        return "Any data"
    }
```
