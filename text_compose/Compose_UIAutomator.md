## ComposeでのUIAutomatorの使用方法 (Ver.2.4.0-alpha05以降)

**UIAutomatorは2.3以前と2.4以降で記述方法が大きく変わっているため注意**

### 依存関係の追加

build.gradle.kts (Module: app)

```kts
    androidTestImplementation("androidx.test.uiautomator:uiautomator:2.4.0-alpha05")
```

### UIの準備

・テスト実施時にidが読み取れるよう、以下のmodifierを記述しておく

1. 一番親となるcomposableに`testTagsAsResourceId = true`を追加
2. テスト時にIdで取得したいcomposableに`Modifier.testTag()` を追加

例

```kotlin
@Composable
fun App(modifier: Modifier = Modifier) {
    var buttonText by remember { mutableStateOf("tap this") }
    val list = List(100) { it.toString() }
    Scaffold(modifier = Modifier.semantics{
        testTagsAsResourceId = true
    }) {
        Column(modifier = Modifier
            .fillMaxSize()
            .padding(it), horizontalAlignment = Alignment.CenterHorizontally) {
            Text("UI Test")
            Button(onClick = {
                buttonText = "taped"
            }) {
                Text(buttonText)
            }
            LazyColumn(modifier = Modifier
                .fillMaxSize()
                .testTag("list")
                .padding(top = 50.dp)) {
                items(list) {
                    Box(modifier = Modifier
                        .fillMaxWidth()
                        .padding(vertical = 4.dp, horizontal = 8.dp)
                        .background(
                            Color.Gray
                        )
                        .clickable {
                            println(it)
                        }, contentAlignment = Alignment.Center) {
                        Text(it)
                    }
                }
            }
        }
    }
}
```

### テストコードの記述

**記述方法がかなり簡略化され、基本的に`uiAutomator{}`関数内に動作を記述する**

例

```kotlin
class ApplicationUITesting {
    @Test
    fun startTesting(){
        uiAutomator {
            startApp("edu.ws2025.a01.composeuiautomatortest")
            Thread.sleep(2000)
            onElement { textAsString() == "tap this" }.click()
            Thread.sleep(2000)
            onElement { viewIdResourceName == "list" }.scrollToElement(direction = Direction.DOWN) { textAsString() == "50" }
            Thread.sleep(2000)
            onElement { textAsString() == "40" }.click()
            Thread.sleep(2000)
        }
    }
}
```

・`startApp()`: 指定したアプリを起動する関数、引数に起動するアプリのパッケージ名を渡す

・`onElement`: UI要素を取得する{}に取得するUIの条件を記述する

- UIに含まれる文字を参照→`textAsString()`
- Composableに振ったId名を参照→`viewIdResourceName`

