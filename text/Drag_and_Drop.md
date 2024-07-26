# Drag & Drop

## Drag

```kotlin
 blueStone.setOnLongClickListener {v ->
    v.startDragAndDrop(null,View.DragShadowBuilder(v),R.drawable.blue_stone,0)
}
```

・第2引数：ドラッグ時のイメージを設定

・第3引数:同時に渡すデータを設定

## Drop

```kotlin
 dropFrame.setOnDragListener { v, event ->
  //ドラッグされたもののデータを取得
  val drawable = event.localState as Int
  //ドロップアクションを取得
  if (event.action == DragEvent.ACTION_DROP){
    val view = ImageView(this@MainActivity)
    view.setImageResource(drawable)
    val imageSize = resources.getDimensionPixelSize(R.dimen.image_size)
    //ドロップされた位置を取得して、Viewの座標に設定
    val params = FrameLayout.LayoutParams(imageSize,imageSize).apply {
      view.x = event.x - imageSize/2
      view.y = event.y -imageSize/2
      }
    view.layoutParams = params
    dropFrame.addView(view)
  }
  true
}
```
