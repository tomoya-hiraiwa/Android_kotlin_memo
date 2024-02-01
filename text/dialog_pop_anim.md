## dialogをポップアップ表示する

1.popupアニメーションファイルの作成(res/anim/dialog_popup.xml)

```
<?xml version="1.0" encoding="utf-8"?>
    <set xmlns:android="http://schemas.android.com/apk/res/android">
        <scale
            android:duration="300"
            android:fromXScale="0.7"
            android:fromYScale="0.7"
            android:pivotX="50%"
            android:pivotY="50%"
            android:toXScale="1.0"
            android:toYScale="1.0" />
        <alpha
            android:duration="200"
            android:fromAlpha="0.0"
            android:toAlpha="1.0" />
    </set>
```

2.ダイアログの作成

・レイアウト

→立体感を出すため、CardViewを使用する、背景色もつける

```kotlin
private fun dialog(){
        Dialog(this).apply {
            val binding = DialogBinding.inflate(layoutInflater)
            setContentView(binding.root)
            //ダイアログの枠の背景を透過して、先に枠だけ見えてしまわないようにする
            window?.setBackgroundDrawable(ColorDrawable(Color.TRANSPARENT))
            val anim = AnimationUtils.loadAnimation(this@MainActivity4,R.anim.dialog_pop)
            binding.root.startAnimation(anim)
        }.show()
    }
```

![pop_dialog](https://github.com/tomoya-hiraiwa/Android_kotlin_memo/blob/main/video/pop_dialog.gif)



