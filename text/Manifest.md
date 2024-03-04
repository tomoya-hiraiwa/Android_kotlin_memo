# Manifest

## uses-feature

### uses-featureとは？

 →GooglePlay配信時に、ここに記述したハードまたはソフトの要件を満たさない端末からそのアプリを表示しないようにする

### 使用例

・カメラ機能の場合

```kotlin
 <uses-feature android:name="android.hardware.camera.any" required="true"/>
```

・name

→必要なハードウェアまたはソフトウェアの情報

・required

→その機能がアプリケーションの動作に必須かどうか(true:必須、false:必須ではない)

### nameタグの名前例

[公式リファレンス](https://developer.android.com/guide/topics/manifest/uses-feature-element?hl=ja)

機能リファレンス欄参照
