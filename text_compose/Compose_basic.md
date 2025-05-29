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

## 監視可能なリストの作成

### Listを作成しておき、remember内で`toMutableStateList`を呼び出す

```kotlin
val list = remember { unobservableList.toMutableStateList() }
```

### `mutableStateListOf`を使用する

・mutableStateOf→`MutableState<T>`を返す

・mutableStateListOf→`SnapshotStateList<T>`を返す

### `SnapshotStateList`

・Compose用のステートを持ったリスト型

・基本的にmutableListなどと同じ使い方ができる

## サイズの指定

### `Modifier.size`を使用する

ex)

```kotlin
 Image(
       painter = painterResource(R.drawable.world_map),
       contentDescription = null,
       modifier
        .size(650.dp)
        .offset(y = 50.dp)
)
```

この場合、親のレイアウトサイズが指定した値より小さい場合、親のサイズに合わされる

### `Modifier.requiredSize`を使用

ex)

```kotlin
 Box(
        modifier = Modifier
            .requiredSize(width = 850.dp, height = 1500.dp)
            .rotate(deg)
            .background(color = colorResource(R.color.pink))
    )
```

こちらは、親のサイズに関わらず必ずこのサイズで表示される
