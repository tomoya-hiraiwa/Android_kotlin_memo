# LineChart

### 基本コード

```kotlin
class LineChart(context: Context, attrs: AttributeSet) : View(context, attrs) {
    var pointList = mutableListOf<Float>()
    var yMax = 100f
    var yCount = 10f
    var xCount = 10f
    var showXLine = true
    var showYLine = true
    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        val sampleSize = canvas!!.width.coerceAtMost(canvas.height)
        val addXSize = sampleSize / xCount
        val addYSize = sampleSize / yCount
        var nowWidth = 0f
        var nowHeight = addXSize * xCount
        for (i in 1 until (xCount + 1).toInt()) {
            canvas.drawLine(nowWidth, 0f, nowWidth, canvas.height.toFloat(), Paint().apply {
                color = Color.GRAY
                strokeWidth = 5f
            })
            nowWidth += addXSize
            if (!showYLine) break
        }
        for (i in 1 until (yCount + 1).toInt()) {
            canvas.drawLine(0f, nowHeight, canvas.width.toFloat(), nowHeight, Paint().apply {
                color = Color.GRAY
                strokeWidth = 5f
            })
            nowHeight -= addYSize
            if (!showXLine) break
        }
        val pointValue = sampleSize / yMax
        if (pointList.isNotEmpty()) {
            for (i in 0 until pointList.size - 1) {
                var fromYValue = sampleSize - pointList[i] * pointValue
                var toYValue = sampleSize - pointList[i + 1] * pointValue
                canvas.drawLine(
                    i * addXSize,
                    fromYValue,
                    (i + 1) * addXSize,
                    toYValue,
                    Paint().apply {
                        color = Color.RED
                        strokeWidth = 10f
                    })
            }
        }
    }
}
```

### 実行例

![LineChart](/photos/LineChart.png)
