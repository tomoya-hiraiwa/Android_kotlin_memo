# Button

・5種類のボタンがある

### プロパティ

・`onClick`:押された時の動作を定義

・`enabled`:ボタンの有効無効を設定

・`colors`:ボタンの色を指定する

・`contentPadding`:ボタン内のpaddingを指定

### 例

```kotlin
@Composable
fun ButtonSample(){
    Row(modifier = Modifier
        .fillMaxWidth()
        .padding(10.dp)) {
       Button(onClick = {}) {
           Text(text = "Filled")
       }
        FilledTonalButton(onClick = {  }) {
            Text(text = "Tonal")
        }
        OutlinedButton(onClick = {  }) {
            Text(text = "Outlined")
        }
        ElevatedButton(onClick = {  }) {
            Text(text = "Elevated")
        }
        TextButton(onClick = {  }) {
            Text(text = "Text")
        }
    }
}
```

![composebutton](/photos/composebutton.png)
