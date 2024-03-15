# MediaStore

## MediaStoreに画像を保存する(Bitmap生成編)

### bitmapを生成する

```kotlin
val frame = b.frame
val bitmap = Bitmap.createBitmap(frame.width,frame.height,Bitmap.Config.ARGB_8888)
val canvas = Canvas(bitmap)
frame.draw(canvas)
```

### MediaStoreに必要な情報を入れる

```kotlin
 val con = ContentValues().apply {
  //画像名
  put(MediaStore.Images.Media.DISPLAY_NAME,"space cat")
  //どんなデータか
  put(MediaStore.Images.Media.MIME_TYPE,"image/png")
}
```

### MediaStoreのuriを取得

```kotlin
val uri = contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI,con)
```

### bitmapを書き込む

```kotlin
contentResolver.openOutputStream(uri!!).use {
  bitmap.compress(Bitmap.CompressFormat.PNG,100,it!!)
  it.flush()
}
```

全文

```kotlin
val frame = b.frame
val bitmap = Bitmap.createBitmap(frame.width,frame.height,Bitmap.Config.ARGB_8888)
val canvas = Canvas(bitmap)
frame.draw(canvas)
val con = ContentValues().apply {
  put(MediaStore.Images.Media.DISPLAY_NAME,"space cat")
  put(MediaStore.Images.Media.MIME_TYPE,"image/png")
}
val uri = contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI,con)
contentResolver.openOutputStream(uri!!).use {
  bitmap.compress(Bitmap.CompressFormat.PNG,100,it!!)
  it.flush()
}
````
