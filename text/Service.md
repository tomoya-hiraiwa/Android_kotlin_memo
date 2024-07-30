# Service

## Foreground Service

### 1.Serviceの作成

```kotlin
class SampleService(): Service() {
    private val channelId = "sample channel"
    private val job = CoroutineScope(Dispatchers.Main)
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        //フォアグラウンドサービスとして登録
        startForeground(1,createNotification())
        job.launch {
            while (true){
                delay(2000)
                println("service is enabled.")
            }
        }
        return START_STICKY
    }
    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
  
    //サービス実行中に表示される通知を作成
    private fun createNotification(): Notification{
        val channel = NotificationChannel(channelId,"Sample channel",NotificationManager.IMPORTANCE_DEFAULT)
        val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.createNotificationChannel(channel)
        val notification = NotificationCompat.Builder(this,channelId).apply {
            setContentTitle("Sample Service")
            setContentText("Service is enabled")
            setSmallIcon(R.drawable.ic_launcher_foreground)
        }.build()
        return notification
    }

    override fun onDestroy() {
        super.onDestroy()
        println("service is disabled")
        job.cancel()
    }
}
```

・`onBind`メソッドは継承必須

・forgroundServiceの場合は通知を表示する必要があるため、`startForeground`メソッドに通知のidと`Notificaton`を渡す

### 2.Actiivtyなどからサービスを呼び出す

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var b: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        b.apply {
            startButton.setOnClickListener {
                val intent = Intent(this@MainActivity,SampleService::class.java)
                startService(intent)
            }
            stopButton.setOnClickListener {
                val intent = Intent(this@MainActivity,SampleService::class.java)
                stopService(intent)
            }
        }
    }
}
```
### 3.Manifestに登録

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
...
  <service android:name=".SampleService"
            android:exported="false"/>
```

##.Serviceのバインド

・`onBind`メソッドにバインダーを登録することでActivityなどからService内のメソッドなどを使用できる

### 1.サービスにバインダーを作成する

```kotlin
 inner class MyBinder: Binder() {
        //サービスをインスタンス化するメソッド
        fun getService(): SampleService = this@SampleService
        //アクティビティからこのメソッドを呼び出して文字列を取得する
         fun getText(): String{
            return "send text from Service."
        }
    }
```
### 2.`onBind`メソッドで作成したバインダーを返す

```kotlin
 override fun onBind(intent: Intent?): IBinder? {
        return MyBinder()
    }
```

### `ServiceConnection`の作成

```kotlin
private val connection = object: ServiceConnection{
        //サービスが作成されたときに呼び出される
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            println("service is connected")
            //バインダーを取得
           val binder = service as SampleService.MyBinder
            //サービスを作成したバインダーのgetService()メソッドで取得
            serv = binder.getService()
        }
        //サービスの接続が解除されたときに呼び出される
        override fun onServiceDisconnected(name: ComponentName?) {
        }

    }
```

### サービスの使用

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var b: ActivityMainBinding
    private lateinit var serv: SampleService
    private val connection = object: ServiceConnection{
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            println("service is connected")
           val binder = service as SampleService.MyBinder
            serv = binder.getService()
        }

        override fun onServiceDisconnected(name: ComponentName?) {
        }

    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        b.apply {
            startButton.setOnClickListener {
                val intent = Intent(this@MainActivity,SampleService::class.java)
                bindService(intent,connection,Context.BIND_AUTO_CREATE)

            }
            stopButton.setOnClickListener {
                //このようにバインダーやサービス内のメソッドを呼び出せる
                println(serv.MyBinder().getText())
                unbindService(connection)
            }
        }
    }
}
```

## サービスとの接続の種類と使い分け

・音楽プレイヤーなど多くの双方向の通信が必要→バインダーを使用

・アクティビティからサービスに命令を送るのみ(一方向)→Intent

・サービスから定期的に値を受け取る→BroadcastReciver
