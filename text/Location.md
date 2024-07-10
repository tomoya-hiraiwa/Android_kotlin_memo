# 座標点の測定

## 1.permissionの設定

```xml
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

・`ACCESS_COARSE_LOCATION`: おおよその現在位置を取得するための権限

・`ACCESS_FINE_LOCATION`: より正確な現在位置を取得するための権限

## 2.座標データの取得

```kotlin
//パーミッションリクエスト用
 private val permissionLauncher =
        registerForActivityResult(ActivityResultContracts.RequestPermission()) { bool ->
            if (bool) {
            }
        }

@RequiresApi(Build.VERSION_CODES.R)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        b.apply {
            permissionLauncher.launch(android.Manifest.permission.ACCESS_FINE_LOCATION)
            //LocationManagerの取得
            val manager = getSystemService(LOCATION_SERVICE) as LocationManager
            //現在の座標を取得する
            manager.getCurrentLocation(
                LocationManager.GPS_PROVIDER,
                null,
                mainExecutor
            ) { location ->
                //値はLocation型で渡される、.latitude,.longitudeで緯度経度を取得できる
                locText.text = "lat: ${location.latitude},lon: ${location.longitude}"
            }


        }
    }
}
```

## 3.継続的に座標を取得する

```kotlin
//定期的に座標を取得するhandlerを作成
 private val locationRunnable = object : Runnable{
        @RequiresApi(Build.VERSION_CODES.R)
        override fun run() {
            getLocation()
            handler.postDelayed(this,20000)
        }
    }

private val permissionLauncher = registerForActivityResult(ActivityResultContracts.RequestPermission()){ bool->
        if (bool){
          //permissionが通ったら実行
          handler.post(locationRunnable)
        }
    }

 private fun getLocation(){
        locationManager = getSystemService(Context.LOCATION_SERVICE) as LocationManager
        locationManager.getCurrentLocation(
            LocationManager.GPS_PROVIDER,
            null,
            mainExecutor
        ){location ->
            println(location.longitude)
            println(location.latitude)
            b.lat.text = "lat: ${location.latitude}"
            b.lon.text=  "lon: ${location.longitude}"
        }
    }

override fun onDestroy() {
        super.onDestroy()
        //画面を削除する時に停止する。
        handler.removeCallbacks(locationRunnable)
    }
```
