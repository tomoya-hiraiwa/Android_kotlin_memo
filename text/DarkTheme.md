# Themeの変更

## darkThemeと通常Themeの変更

```kotlin
//ダークテーマの場合
AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES)
//ライトテーマの場合
AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO)
```

## Themeの状態の取得

```kotlin
 val conf = requireActivity().resources.configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
        if (conf == Configuration.UI_MODE_NIGHT_YES){
           //ダークモードだった時の処理
        }
        else //通常モードだった時の処理
```
