# StatusBar

## StatusBarの色を変更する

### Step1

・レイアウトファイルのルート要素のViewに`android:fitsSystemWindows="true"`を追加

例

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp"
    android:fitsSystemWindows="true"
    tools:context=".LoginActivity">
```

### Step2

・コードで色を指定

例

```kotlin
window.statusBarColor = ContextCompat.getColor(this, R.color.orange)
```

・透過したい場合

```kotlin
window.statusBarColor = Color.TRANSPARENT
```

> [!Tip]
> xmlでレイアウトする時、通常はStatusBarの分下げてレイアウト定義されるため、画像などを被せたい場合、
> `android:layout_marginTop="-30dp"`などをしてレイアウトを明示的に上に上げる必要がある

## StatusBarを非表示にする

> [!CAUTION]
> 現在このコードはdepricated

```kotlin
 window.decorView.systemUiVisibility = View.SYSTEM_UI_FLAG_FULLSCREEN
        actionBar?.hide()
```
