# List

## リスト内のある変数の合計値を求める

```kotlin
list.sumOf{...}
```

例：商品の合計の値段を求める

```kotlin
totalPrice = shopList.sumOf { it.price }
```

## `size`と`indices`の違い

### `size`

・`Int`型

・リストに含まれる要素の数を返す

・必ず正の値

```kotlin
//リストに3つの要素があった場合
val listSize = list.size //3
//リストが空のとき
val listSize = list.size //0
//for文で使いたい時
for(i in 0 until datalist.size){} //iは0からdatalist.size-1の値まで繰り返す
```

### indices

・`IntRange`型(a..b)の範囲を返す形

・インデックスの範囲を返す

・リストが空の時は`0..-1`という形で返す

```kotlin
//リストに3つの要素があった場合
val listSize = list.indices //0..2
//リストが空のとき
val listSize = list.indices //0..-1
//for文で使いたい時
for(i in list.indeces){} //リストのインデックス範囲分繰り返す
```

## リストからランダムな値をn個取得する

### リストの順番を入れ替えてから`take`メソッドを用いて取得する

```kotlin
 val randomColors = selectColorData.shuffled().take(3)
```


