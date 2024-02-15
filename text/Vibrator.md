## Vibrator

・VibratorManagerを使用する

### 一度だけバイブする場合

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var b: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S){
        val vibe = getSystemService(Context.VIBRATOR_MANAGER_SERVICE) as VibratorManager
        val vibrator = VibrationEffect.createOneShot(300,VibrationEffect.DEFAULT_AMPLITUDE)  //300ms間バイブレーション
        val combine = CombinedVibration.createParallel(vibrator)
        vibe.vibrate(combine)
        }
    }
}
```

###　指定回数バイブレーションする場合

```kotlin
val vibrator = VibrationEffect.createWaveform(longArrayOf(1000,1000), intArrayOf(0,VibrationEffect.DEFAULT_AMPLITUDE),0)
```

・craeteWaveFromメソッド

第1引数：各バイブレーションごとの秒数(longArray)

第2引数：各バイブレーションの大きさ(intArray)

 ・0→バイブレーションなし

第3引数：繰り返す際の開始地点のインデックス

　・-1→繰り返しなし
