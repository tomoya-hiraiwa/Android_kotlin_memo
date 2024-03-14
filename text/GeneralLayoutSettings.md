# レイアウト全般設定

## `LayoutParams`について

・親のViewに合わせたLayoutParamsを使用する

例：
  ・親が`LinearLayout`→`LinearLayout.LayoutParams`
  ・親が`FrameLayout`→`FrameLayout.LayoutParams`

・違うレイアウトのものを使用すると`ClassCastException`が発生する
