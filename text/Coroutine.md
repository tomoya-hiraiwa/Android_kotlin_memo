# kotlin Coroutine

## 最初のコルーチン

```kotlin
import kotlinx.coroutines.*

fun main(args: Array<String>) = runBlocking {
	launch {
        delay(1000L)
        println("World")
    }
    println("Hello")
}
```

・結果

```
Hello
World
```

・`launch`: コルーチンを起動し、このスコープ内のコードは独立して処理される

・`delay`: コルーチンを特定の時間中断する

・`runBlocking`: 非コルーチン部分とコルーチンを結びつけるコルーチンビルダーの一種

> [!CAUTION]
> `runBlocking`はスレッド内のすべてのコルーチンが終了するまでスレッドをブロックするため、
> 基本的に使用しない

<br>

## 関数化

・`suspend`修飾子を使用する

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
		launch{ doWorld() }
        println("Hello")
}

suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
```

<br>

## スコープを作成する

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    doWorld()
}

suspend fun doWorld() = coroutineScope {
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello")
}
```

・独自のコルーチンスコープを作成する。定義されたすべての子の動作が完了するまで終了しない

・`corutineScope`: `runBlocking`と似ているが、こちらはスレッドをブロックしない → susupend関数になる

例

```kotlin
fun main() = runBlocking { //こちらは通常の関数のためOK
    doWorld()
}

fun main() = coroutineScope { //suspend関数のためNG
    doWorld()
}
```

・コードを検索すると...

runBlocking: `public actual fun <T> runBlocking`

coroutineScope: `public suspend fun <R> coroutineScope`

<br>

## 複数のコルーチンを起動する

```kotlin
fun main() = runBlocking {
    doWorld()
    println("Done")
}

suspend fun doWorld() = coroutineScope { 
    launch {
        delay(2000L)
        println("World 2")
    }
    launch {
        delay(1000L)
        println("World 1")
    }
    println("Hello")
}
```

・結果

```
Hello
World 1
World 2
Done
```

## Job

・コルーチンビルダー(launchなど)は、起動したコルーチンの参照を持つ`Job`オブジェクトを返す

・`.join()`を使用することで、コルーチンの終了を待機させてから、その後の動作を行う

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {

    val job = launch { 
        delay(1000L)
        println("World!")
    }
    //job内のdelay(1000L)で待機している間に"Hello"が出力
    println("Hello")
    //jobの完了を待つ　→ これより下のコードはjobが完了するまで実行されない
    job.join()
    println("Done")
}
```

・結果

```
Hello
World!
Done
```
<br>

## 軽量

・`Thread`を使用する場合とは違い、コルーチンは軽量のため、より多くの非同期処理を実行できる

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    repeat(50_000) { //50000個のコルーチンを起動する
        launch {
            delay(5000L)
            print(".")
        }
    }
}
```

<br>

## スレッドの切り替え

・`dispatcher`により、スレッドを切り替えることができる

・種類

|名称|役割|
|:-:|:-:|
|`Dispatchers.Main`|メインスレッドで実行、UI処理に関するもの、suspend関数の呼び出しなど|
|`Dispatchers.IO`|ファイルの読み書き、ネットワーク関係、Roomの使用など|
|`Dispathcers.Default`|Jsonの解析、その他データフィルタ、ソートなどの負荷の高い作業|

・切り替えの例

```kotlin
suspend fun get(url: String) =                 // Dispatchers.Main
    withContext(Dispatchers.IO) {              // IOスレッドに変更
      //この中の処理はIOスレッドで行われる
    }
  //ここはメインスレッドに戻る                                    
}
```
<br>

## Scopeのキャンセル

・`.cancel()`を呼び出すことでコルーチンのキャンセルができる

・一度止めたコルーチンを再開することはできない（再作成する）

例 `onResume`が呼ばれた時にコルーチンを起動し、`onPause`で停止させる　→ アクティビティ表示中に起動する

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var parentScope: Job
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
    override fun onResume() {
        super.onResume()
        parentScope = CoroutineScope(Dispatchers.Main).launch {
            launch(Dispatchers.Default) {
                while (true) {
                    delay(2000)
                    println("coroutine is worked.")
                }
            }
        }
    }
    override fun onPause() {
        super.onPause()
        println("call onPause")
        parentScope.cancel("Scope is canceled.")
    }
}
```
