# 画面の向きによるレイアウト変更

## レイアウトファイルから変更する場合

### layout-landをdirectoryに指定し、元のレイアウトファイルと同じ名前でxmlファイルを作成する

・MainActivityの場合

![layout_land](/photos/land_layout.png)

### idの扱い

・どちらのレイアウトも共通のidを指定している場合

→non nullのとして定義される

・片側のレイアウトのみに存在する要素があるorそれぞれでidが違う

→nullableとして定義される

## 画面の向きを取得する

```kotlin
//画面の向きを取得
 val orientation = resources.configuration.orientation
if (orientation == Configuration.ORIENTATION_PORTRAIT){
  //縦向きの時の処理
}
else{
  //横向きの時の処理
}
```
