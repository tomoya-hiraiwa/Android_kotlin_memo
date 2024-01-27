# TransistionManager

・画面レイアウトの変更時にアニメーションをかけることができる

## TransistionManager.beginDelayedTransition(ViewGroup)

・画面に要素が追加、削除される時に展開、縮小するようなアニメーションがつく

・用途例：展開するリスト、メニュー項目の作成時、表示アイテム追加時など

例：TextViewのVisibilityをVisible,Goneで表示非表示の例

```kotlin
 binding.button.setOnClickListener {
            isChecked = !isChecked
            TransitionManager.beginDelayedTransition(binding.root)
            if (isChecked){
                binding.text2.visibility = View.VISIBLE
            }
            else binding.text2.visibility = View.GONE
        }
    }
```
