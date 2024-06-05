# File

## Fileを作成する

```kotlin
//親ファイルパスとファイル名を指定
val file = File(filesDir,"fileName")

//ファイルの存在確認→なければ作成
if(!file.exists()){
  file.createNewFile()
}
```

・fileが存在しない時のみ新たなファイルを作成することで、ファイル自体を上書きしないようにする

## assetsから取得したファイルの内容をコピーする

```kotlin
val input = assets.open("sample_data.json")

val file = File(filesDir,"fileName")

if(!file.exists()){
  file.outputStream().use{input.copyTo(it)} //it→file.OutputStream
}
```

## ファイルのデータを取得

```kotlin
val data = file.bufferdReader().use{it.readText()}
```

## ファイルにデータを追加

```kotlin
val addData = "Somothing add content here."

file.outputStream().use {
  it.write(addData.toByteArray())
  it.flush()
}
```

>[!NOTE]
>データは上書きなので、元データに追加の場合、追加するデータのみではなく、すでに入っていたデータも含めて投げる

## 共有ストレージにファイルを保存する

### Intentの作成

```kotlin
   private fun shareFile(){
        val intent = Intent(Intent.ACTION_CREATE_DOCUMENT).apply {
            addCategory(Intent.CATEGORY_OPENABLE)
            //ファイルのタイプを指定
            type = "text/plain"
            //保存画面で初期に入力されるファイル名を指定
            putExtra(Intent.EXTRA_TITLE,"data.txt")
        }
        saveLauncher.launch(intent)
    }
```

### ランチャーの作成

```kotlin
val saveLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){res ->
        if (res.resultCode == Activity.RESULT_OK){
            res.data?.data?.let {
                //ファイルの保存に成功したら、データを書き込む
                val addData = gson.toJson(currentData)
                contentResolver.openOutputStream(it)?.use {
                    it.write(addData.toByteArray())
                    it.flush()
                }
            }
        }
    }
```







