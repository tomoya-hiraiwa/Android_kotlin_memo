## Zebura crossing

### QRコードを読み取る例

### 1.build.gradle

```kotlin
implementation("com.journeyapps:zxing-android-embedded:4.3.0")
```

### 2.permission

```xml
 <uses-feature
        android:name="android.hardware.camera"
        android:required="false" />
    <uses-permission android:name="android.permission.CAMERA"/>
```

### 3.permissionの確認

```kotlin
 //パーミッションランチャー呼び出し
        permissionLauncher.launch(android.Manifest.permission.CAMERA)
```

```kotlin
 private val permissionLauncher = registerForActivityResult(ActivityResultContracts.RequestPermission()){_ ->}
```

### 4.QR読み取り

```kotlin
 binding.button.setOnClickListener {
            //オプション指定
            val options = ScanOptions().apply {
                setOrientationLocked(false)
                //読み取る対象をQRのみに指定
                setDesiredBarcodeFormats(ScanOptions.QR_CODE)
            }
            //カメラ起動
            barcodeLauncher.launch(options)
        }
```

```kotlin
//カメラアプリ起動、読み取り後の処理
    val barcodeLauncher = registerForActivityResult(
        ScanContract()
    ) { result ->
        if (result.contents == null) {
            // ここでQRコードを読み取れなかった場合の処理を書く
            Toast.makeText(this, "Cancelled", Toast.LENGTH_LONG)
                .show()
        } else {
            // ここでQRコードを読み取れた場合の処理を書く
        }
    }
```
