# BarChart

## TextViewで作成する

### `backGround`に色を設定しておき、View幅でグラフのように見せる

例

```xml
 <TextView
        android:id="@+id/graph"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:background="@color/black" />
```

```kotlin
b.graph.layoutParams = LayoutParams(graphWidth, LayoutParams.WRAP_CONTENT) //graphWidth: グラフの長さ
```

### 増加アニメーションをつける場合

```kotlin
val scope = CoroutineScope(Dispatchers.IO)
            scope.launch {
                var width = 0
                while (width < max){
                    width += 10
                    withContext(Dispatchers.Main) {
                        b.graph.layoutParams = LayoutParams(width, LayoutParams.WRAP_CONTENT)
                    }
                    delay(20)
                }
            }
            b.graph.layoutParams  = LayoutParams(data.medal_point* 10,LayoutParams.WRAP_CONTENT)
}
```
