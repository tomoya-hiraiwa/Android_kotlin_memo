# EditText

### 選択時にキーボードを表示しない

```kotlin
EditText.isFocusable = falese
```

## KeyListener

### キーボードのEnter(☑️)を押した時のリスナを設定

```kotlin
 b.textEdit.setOnKeyListener { v, keyCode, event ->
    if (keyCode == KeyEvent.KEYCODE_ENTER && event.action == KeyEvent.ACTION_DOWN){
     println(b.textEdit.text.toString())
   }
  true
}
```
