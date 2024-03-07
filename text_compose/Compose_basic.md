# Jetpack Composeの基本

## 1.Composableな関数

・`@Composable`アノテーションがついた通常の関数

・UIの作成を行うことができる

 例

 ```kotlin
@Composable
fun Greeting(modifier: Modifier = Modifier) {
    Text(
        text = "Hello World",
        modifier = modifier
    )
}
```

この例であれば、"Hello World"と表示するTextのみがあるComposableな関数となっている

## 2.アプリのエントリーポイント

・`MainActivity`がエントリーポイントであることは変わらない

・`setContent`内でComposableな関数を呼び出してレイアウトを作成する

例（Greeting関数を使用して、Hello Worldをアプリに表示）

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            WSC2022SE_Compose_TestTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    Greeting()
                }
            }
        }
    }
}
```

<p align="center">
<img src="photos/compose_helloworld.png" width="200px">
</p>

## 3.Previewの利用

・Previewを利用するには`@Preview`アノテーションをつけた関数を作成する

例

```kotlin
@Preview(showBackground = true)
@Composable
fun GreetingPreview(){
    Greeting()
}
```

・`showBackground`属性：背景を表示するかどうか

<p align="center">
<img src="photos/compose_preview.png">
</p>

[次：UIの修正 >](text_compose/Compose_basic_2.md)
