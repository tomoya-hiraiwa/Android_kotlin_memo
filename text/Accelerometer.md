# 加速度センサによる端末の傾きの取得

### SensorEventListenerのインターフェースを継承する


例:

```kotlin
class MainActivity : AppCompatActivity(), SensorEventListener {
    private lateinit var b: ActivityMainBinding
    private lateinit var sensorManager: SensorManager
    private var accelerometer: Sensor? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        //SensorManagerの取得
        sensorManager = getSystemService(SENSOR_SERVICE) as SensorManager
        //加速度センサの取得
        accelerometer = sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER)
    }

    override fun onResume() {
        super.onResume()
        //画面が呼ばれた時にsensorManagerに加速度センサを登録
        accelerometer?.also { sensor ->
            sensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_GAME)
        }

    }
  //必須override、センサの値が変更された時の動作
    override fun onSensorChanged(event: SensorEvent?) {
        event?.let {
            when (it.sensor.type) {
                Sensor.TYPE_ACCELEROMETER -> {
                    //値の取得
                    val x = it.values[0]
                    val y = it.values[1]
                    val z = it.values[2]
                    b.accelerometerText.text = "Accelerometer\nx: $x\ny: $y\nz: $z"
                    //今回の場合は端末を左に傾けるとViewが左に、右に傾けるとViewが右に動く
                        if (x > 2.0f && b.view.x > -0f + b.view.width/2){
                            b.view.x -=10f
                            b.view.invalidate()
                        }
                    else if (x< -2.0f && b.view.x < b.root.width - b.view.width-48f){
                            println(b.root.width - b.view.width-16f)
                            b.view.x +=10f
                            b.view.invalidate()
                    }
                }
            }
        }
    }
    //センサの精度が変わった時の動作
    override fun onAccuracyChanged(sensor: Sensor?, accuracy: Int) {

    }
    //画面が停止する時にセンサの登録を解除する
    override fun onPause() {
        super.onPause()
        sensorManager.unregisterListener(this)
    }
}
```

・基本的に手に持った状態での左右の傾きはxの値で検知できる
