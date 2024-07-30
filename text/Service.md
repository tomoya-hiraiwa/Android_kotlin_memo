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
