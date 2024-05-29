# レイアウト全般設定

## `LayoutParams`について

・親のViewに合わせたLayoutParamsを使用する

例：
  ・親が`LinearLayout`→`LinearLayout.LayoutParams`
  ・親が`FrameLayout`→`FrameLayout.LayoutParams`

・違うレイアウトのものを使用すると`ClassCastException`が発生する

## `Float`の値を`dp`に変換する

```kotlin
val dp = newVal * resources.displayMetrics.density //newVal: Float値
```
