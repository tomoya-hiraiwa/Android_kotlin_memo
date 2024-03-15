# ShareSheet

## テキストを他のアプリなどと共有する

### 基本コード

```kotlin
  val intent = Intent().apply {
            action = Intent.ACTION_SEND
            putExtra(Intent.EXTRA_TEXT,"test")
            type = "text/plain"
        }
        startActivity(Intent(Intent.createChooser(intent,null)))
```
