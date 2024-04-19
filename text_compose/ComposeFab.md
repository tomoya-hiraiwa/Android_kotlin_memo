# FAB

・4種類のFABがある

大、中、小の3サイズ+テキストなども含んだ拡張FAB

### パラメータ

・`onClick`:押された時の動作

・`containerColor`:ボタンの色

・`contentColor`:アイコンの色

## 例

```kotlin
@Composable
fun FabSample(){
    Row(modifier = Modifier
        .fillMaxWidth()
        .padding(10.dp)) {
        FloatingActionButton(onClick = { /*TODO*/ }, modifier = Modifier.padding(end = 8.dp)) {
            Icon(imageVector = Icons.Filled.Add, contentDescription = null)
        }
        SmallFloatingActionButton(onClick = { /*TODO*/ },modifier = Modifier.padding(end = 8.dp)) {
            Icon(imageVector = Icons.Filled.Edit, contentDescription = null)
        }
        LargeFloatingActionButton(onClick = { /*TODO*/ }, modifier = Modifier.padding(end = 8.dp)) {
            Icon(imageVector = Icons.Filled.Delete, contentDescription = null)
        }
        FloatingActionButton(onClick = { /*TODO*/ },
            modifier = Modifier.padding(end = 8.dp),
            shape = CircleShape) {
            Icon(imageVector = Icons.Default.Add, contentDescription = null)
        }
        ExtendedFloatingActionButton(onClick = { /*TODO*/ },
            icon = { Icon(imageVector = Icons.Filled.Edit, contentDescription = null )},
            text = { Text(text = "Edit")})
    }
}
```

![fab](/photos/fab.png)



