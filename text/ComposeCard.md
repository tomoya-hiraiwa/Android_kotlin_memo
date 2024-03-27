# Card

・`Scaffold`は画面全体をカバーするようなコンテナであることに対し、`Card`はより小規模なコンテナ

### パラメータ

・`elevation`:影をつけて、浮き上がっているように見せる

・`colors`:`CardColors`を使用し、コンテナおよび子の色を指定する

・`enabled`:有効無効の設定

## 基本実装

```kotlin
fun NormalCard(){
    Card() {
        Text(text = "Normal card",
            modifier = Modifier.padding(10.dp))
    }
}
```

## 応用実装

```kotlin
@Composable
fun CardSample(){
    Column() {
        //Filled
        Card(
            colors = CardDefaults.cardColors(
                containerColor = MaterialTheme.colorScheme.secondaryContainer
            ), modifier = Modifier
                .padding(8.dp)
                .width(200.dp)
                .height(100.dp)
        ) {
            Text(text = "Filled", modifier = Modifier.padding(8.dp))
        }
        //Elevated
        ElevatedCard(
            elevation = CardDefaults.cardElevation(
                defaultElevation = 6.dp
            ),
            modifier = Modifier
                .padding(8.dp)
                .width(200.dp)
                .height(100.dp)
        ) {
            Text(text = "Elevated", modifier = Modifier.padding(8.dp))
        }
        //Outlined
        OutlinedCard(
            border = BorderStroke(1.dp,Color.DarkGray),
            modifier = Modifier
                .padding(8.dp)
                .width(200.dp)
                .height(100.dp),
        ) {
            Text(text = "Outlined", modifier = Modifier.padding(8.dp))
        }
    }
}
```

![card](/photos/card.png)
