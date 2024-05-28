# グラデーション付きラインインジケータ

```kotlin
class IndicatorView(context: Context, attrs: AttributeSet): View(context,attrs) {
    //実際の値を受ける
    var value = 0f
    //ベースの線用のPaint
    private val basePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.WHITE
        strokeWidth = 30f
        style = Paint.Style.FILL
        strokeCap  = Paint.Cap.ROUND
    }
    //実際の値の線用のPaint
    private val valuePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        strokeWidth  = 30f
        strokeCap = Paint.Cap.ROUND
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        //グラデーションの準備
        val colors = intArrayOf(context.getColor(R.color.g1),context.getColor(R.color.g2),context.getColor(R.color.g3))
        val positions: FloatArray? = floatArrayOf(0f,0.5f,1f)
        //実際の線の長さを決める (全体長さ) / 最大値 * 実際の値
        val drawValue = ((canvas!!.width/2f +400) / 100 * value)
        val linearGradient = LinearGradient(0f,0f,canvas.width /2 +400f,0f,colors,positions,Shader.TileMode.MIRROR)
        valuePaint.shader = linearGradient
        //ベースの線を描画
        canvas.drawLine(canvas.width /2f - 400f, canvas.height /2f,canvas.width /2f + 400f,canvas.height/2f,basePaint)
        //実際の値の線を描画
        canvas.drawLine(canvas.width /2f - 400f, canvas.height /2f,drawValue,canvas.height/2f,valuePaint)
    }
}
```

## 改良版（サイズ変更など対応）

```kotlin
class GradientIndicator(context: Context,attrs: AttributeSet): View(context,attrs) {
    private val max = 100000f
    var now = 0f
    private val basePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        strokeCap = Paint.Cap.ROUND
        strokeWidth = 50f
        style = Paint.Style.FILL
        color = Color.GRAY
    }
    private val valuePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        strokeWidth = 50f
        strokeCap = Paint.Cap.ROUND
        style = Paint.Style.FILL
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        val colors = intArrayOf(context.getColor(R.color.g1),context.getColor(R.color.g2),context.getColor(R.color.g3))
        val positions = floatArrayOf(0f,0.5f,1f)
        //グラデーションのスタート、終了地点はView描画の座標と合わせる
        val gradient = LinearGradient(canvas!!.width /2f -300f,0f,canvas!!.width /2f + 300f,0f,colors,positions,Shader.TileMode.MIRROR)
        valuePaint.shader = gradient
        //Viewの最大長さ座標-Viewの始点座標で全体の長さを求め、そのうちの割合を求める
        val value = ((canvas.width /2f + 300f) - (canvas.width /2f -300f)) / max * now
        canvas.drawLine(canvas.width / 2f - 300f,canvas.height /2f,canvas.width /2f + 300f, canvas.height /2f,basePaint)
        //Viewの終端座標はViewの始点座標+value
        canvas.drawLine(canvas.width / 2f - 300f,canvas.height /2f,canvas.width /2f -300f + value, canvas.height /2f,valuePaint)
    }
}
```

![indicator](/photos/gr_l_ind.png)
