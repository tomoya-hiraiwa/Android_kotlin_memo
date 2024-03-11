# Base64

## 画像データをBase64エンコード

### `ByteArray`の形にして変換する

```kotlin
//保存したい画像をBitmapとして取得
val image = BitmapFactory.decodeResource(resources,R.drawable.wall_paper)
//書き込むためのByteArrayOutputStreamを準備
val stream = ByteArrayOutputStream()
//準備したByteArrayOutPutStreamに画像を突っ込む
image.compress(Bitmap.CompressFormat.PNG,50,stream)
//ByteArrayに変換
val byte = stream.toByteArray()
//エンコード
val encodeString = Base64.encodeToString(byte,Base64.DEFAULT)
```

## 画像データをBase64デコード

### decodeしたByteArrayを`BitmapFactory.decodeByteArray`でBitmapにする

```kotlin
//decode(data.seatType:デコードしたい文字列データ)
val decodeByte = Base64.decode(data.seatType,Base64.DEFAULT)
val bitmap = BitmapFactory.decodeByteArray(decodeByte,0,decodeByte.size,options)
b.imageView.setImageBitmap(bitmap)
```
