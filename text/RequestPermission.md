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

### 実装例

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ActivityCompat.requestPermissions(this, REQUEST_PERMISSIONS, REQUEST_CODE)
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        if (requestCode == REQUEST_CODE){
            for (permission in permissions){
                if (!checkPermission(permission)){
                    Toast.makeText(this, "permission failed.", Toast.LENGTH_SHORT).show()
                    return
                }
            }
            Toast.makeText(this, "permission completed", Toast.LENGTH_SHORT).show()
        }
    }
    private fun checkPermission(permission: String): Boolean{
        return ContextCompat.checkSelfPermission(baseContext,permission) == PackageManager.PERMISSION_GRANTED
    }
```


