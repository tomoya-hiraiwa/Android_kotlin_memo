# 権限リクエスト

## 複数の権限をまとめてリクエスト

### ActivityCompat.requestPermissions

```kotlin
ActivityCompat.requestPermissions(
  activity: Activity,
  permissions: Array<String!>, //リクエストする権限のリスト
  requestCode: Int //リクエストの識別コード
)
```

・このメソッドを呼びだすとリクエストの結果がonRequestPermissionsResultに配信される

### onRequestPermissionResult

```kotlin
onRequestPermissionResult(
  requestCode: Int, //リクエスト識別コード
  permissions: Array<String>, //リクエスト名
  grantResults: IntArray //権限の検証結果
)
```

### 
