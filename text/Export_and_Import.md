# ファイルのExport,Import

## ファイルをエクスポートする

### 1.intentの作成

```kotlin
  val intent = Intent(Intent.ACTION_CREATE_DOCUMENT).apply {
                addCategory(Intent.CATEGORY_OPENABLE)
                //エクスポートするファイルの形式に合わせて設定
                type = "application/json"
                //初期表示のファイル名を設定
                putExtra(Intent.EXTRA_TITLE,"password.json")
            }
```

### 2.Launcherの作成

```kotlin
  private val exportPasswordLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){res ->
        if (res.resultCode == Activity.RESULT_OK){
            res.data?.data?.let { uri ->
                //作成したファイルにデータを書き込む
                contentResolver.openOutputStream(uri)?.use {
                    it.write(gson.toJson(allPassDataList).toByteArray())
                    it.flush()
                }
                Toast.makeText(this, "Exported Json file.", Toast.LENGTH_SHORT).show()
            }
        }
    }
```

## ファイルをインポートする

### 1.intentの作成

```kotlin
private fun importData(){
        val intent = Intent(Intent.ACTION_OPEN_DOCUMENT).apply {
            addCategory(Intent.CATEGORY_OPENABLE)
            //開きたいファイル形式のものにあわせる
            type = "application/json"
        }
        importPasswordLauncher.launch(intent)
    }
```

### 2.Launcherの作成

```kotlin
private val importPasswordLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){res ->
        if (res.resultCode == Activity.RESULT_OK){
            res?.data?.data.let { uri ->
                val input = contentResolver.openInputStream(uri!!)
                val inputData = input!!.bufferedReader().use { it.readText() }
                if(inputData.isNotEmpty()){
                    //gsonでデータを変換する場合、正しい形式でない場合はエラーの可能性があるのでtryを使用
                    try {
                        //開いたファイルからデータを取得してきて、アプリ内のローカルファイルに追加する
                        val passDataList = gson.fromJson<MutableList<PasswordItem>>(inputData,object : TypeToken<MutableList<PasswordItem>>(){}.type)
                        allPassDataList.addAll(passDataList)
                        passFile.outputStream().use {
                            it.write(gson.toJson(allPassDataList).toByteArray())
                            it.flush()
                        }
                        supportFragmentManager.beginTransaction()
                            .replace(R.id.homeContainerView,PasswordFragment())
                            .commit()
                    }
                    catch (e: Exception){
                        println("$e")
                        Toast.makeText(this, "import failed.", Toast.LENGTH_SHORT).show()
                    }
                }
            }
        }
    }
```
