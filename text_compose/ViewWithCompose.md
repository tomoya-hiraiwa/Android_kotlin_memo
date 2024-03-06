# ViewとComposeの同時使用

## ViewベースにComposeを組み合わせる

### 依存追加

```kotlin
buildFeatures{
        compose = true
    }
```

```kotlin
implementation("androidx.compose.material3:material3:1.2.0")
implementation("androidx.activity:activity-compose:1.8.2")
```

・Syncエラーが出る場合はエラーメッセージに合わせてbuild.gradle(Project)内のkotlinのVerを変更する


### レイアウトの作成

```xml
 <androidx.compose.ui.platform.ComposeView
        android:layout_marginTop="10dp"
        android:id="@+id/comp_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

・ComposeViewを追加する

### Viewの作成

・アクティビティにComposeViewを取り付ける

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var b: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        b.compView.apply {
            setViewCompositionStrategy(ViewCompositionStrategy.DisposeOnViewTreeLifecycleDestroyed)
            setContent {
                //Composeの内容
                MaterialTheme {
                    homeList()
                }
            }
        }
    }
    
}
```

・UIパーツはそれぞれ`@Composable`をつけた関数で作成する

## Previewの使用

### 依存追加

```kotlin
implementation("androidx.compose.ui:ui-tooling-preview:1.6.2")
debugImplementation("androidx.compose.ui:ui-tooling:1.6.2")
```

・使用方法はいつも通り、`@Preview`アノテーションをつける
