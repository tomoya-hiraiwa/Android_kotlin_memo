# Picture in Pictureモードの使用

## 1.Manifestファイルの設定

```xml
<activity
            android:name=".MainActivity"
            android:supportsPictureInPicture="true"
            android:configChanges="screenSize|smallestScreenSize|screenLayout|orientation"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

・`supportsPictureInPicture`を`true`に設定する

・`configChanges`で画面サイズが変わった時にアクティビティを再生成しないようにする

## 2.PIPの実行

```kotlin
//アクティビティ内でオーバーライド
override fun onUserLeaveHint() {
        enterPictureInPictureMode()
    }
```
