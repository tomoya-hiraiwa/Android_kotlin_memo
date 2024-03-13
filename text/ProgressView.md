# ProgresView

## 進捗度などを表すProgressViewを独自実装する

### 完成イメージ

![progress_view](/photos/progress_view.png)

## Viewクラスの作成

```kotlin
class CircleView(context:Context, attributeSet: AttributeSet): View(context,attributeSet){
    var goal = 100f
    var value = 40f

    val paint = Paint().apply {
        strokeWidth = 10f
        strokeCap = Paint.Cap.ROUND
        style = Paint.Style.STROKE
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        var percent = 360/goal*value
        println(percent)
        val padding = 30f
        val rectF = RectF(padding,padding,canvas.width-padding,canvas.height-padding)
        canvas.drawCircle((padding + canvas.width - padding)/2,(padding + canvas.height - padding)/2,(-padding + canvas.height - padding)/2,paint.apply { color = ContextCompat.getColor(context,R.color.back_gray) })
        canvas.drawArc(rectF,-90f,percent,false,paint.apply { color = Color.RED })
    }
}
```

### 解説

・変数`goal`および`value`: `goal`はmaxの値を、`value`は現在の値を定義する

・`paint`変数:描画時の線の基本設定を定義する
  ・`strokeWidth`:線の太さを指定
  ・`strokeCap`:線の端の形を指定
  ・`style`:塗り方を指定

・`percent`:円弧をどこまで描画するかを決める

360度 / `goal`(maxの値) * `value`(現在地)

・`padding`:canvasにpaddingをつけるため、その幅を定義

・`rectF`:円弧の描画範囲を決める`left`,`top`,`right`,`bottom`の座標取り

・`canvas.drawCircle`:下地のグレーの縁を描画

・`caonvas.drawArc`:進捗の円弧を描画

### ポイント
・`canvas.drawArc`のスタートアングルは-90
・Arcで出した座標をもとに、Circleん中心および半径を指定

### 数値を徐々に増やしてアニメーションにする

例

```kotlin
lifecycleScope.launch {
           while (count < maxCount){
               count +=1
               b.circle.graph.value = count.toFloat()
               b.circle.graph.invalidate()
               delay(20)
           }
       }
```
