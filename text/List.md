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
