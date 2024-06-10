# グラデーション

## XMLから背景としてグラデーションを作成する

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
<gradient android:angle="270"
    android:startColor="#81D4FA"
    android:endColor="#ffffff"/>
</shape>
```

`angle`:グラデーションする向き（角度）を指定

`startColor`,`endColor`:グラデーションを始める色と最終的な色をそれぞれ指定

## shaderを作成する

```kotlin
 val colors = intArrayOf(context.getColor(R.color.graph_rise),context.getColor(R.color.graph_top),context.getColor(R.color.graph_set))
        val positions = floatArrayOf(0f,0.5f,1f)
        val sweepGradient = SweepGradient(canvas!!.width /2 .toFloat(), canvas.height /2 .toFloat(), colors,positions)
        //Paintのshaderとして指定
        frontPaint.shader = sweepGradient
```

・`intArray`でグラデーションに使用する色を指定する(何色でもいい)

  例の場合、rise(黄)→top(オレンジ)→set(赤)のようにグラデーションする

・`floatArray`で各色のグラデーションんタイミングを指定

  使用する色の個数と同じ分指定

  値は0.0f~1.0fの間で指定

  →等分になるように指定すると、各色がそれぞれ同じ間隔でグラデーションされる

・`SweepGradient`でグラデーションを作成

  1,2引数:グラデーションの中心位置を指定する

  3引数:使用する色のリストを指定

  4引数:グラデーション間隔のリストを指定


  ## LinearGradient

  ・直線的なグラデーションをする際に使用する

  ### 例

  ```kotlin
 val colors = intArrayOf(context.getColor(R.color.g1),context.getColor(R.color.g2),context.getColor(R.color.g3))
        val positions: FloatArray? = floatArrayOf(0f,0.5f,1f)
        val drawValue = ((canvas.width/2f +400) / 100 * value)
        println(drawValue)
        val linearGradient = LinearGradient(0f,0f,canvas.width /2 +400f,0f,colors,positions,Shader.TileMode.MIRROR)
        valuePaint.shader = linearGradient
        canvas.drawLine(canvas.width /2f - 400f, canvas.height /2f,canvas.width /2f + 400f,canvas.height/2f,basePaint)
        canvas.drawLine(canvas.width /2f - 400f, canvas.height /2f,drawValue,canvas.height/2f,valuePaint)
```

・使用する色のリストを作成する、色変更のポジションを指定するのはSweepGradientと同じ

・第1,2引数にスタートの座標を渡す

・第3,4引数にグラデーション終了の座標を渡す

## 各Gradientクラスの違い

### LinearGradient

・直線的なグラデーションに使用

・水平、垂直、斜めなどの角度で直線的にグラデーション

### RadialGradient

・放射状のグラデーション

・中心点から放射状にグラデーションされる

### SweepGradient

・円周に沿ったグラデーション

・中心点から時計回りにグラデーションされる

## SweepGradientの使用例

```kotlin
 gradient = SweepGradient(
            //中心点x,y
            w / 2f, h / 2f,
            //グラデーションの色,割合
            colors, positions,
        )
        //開始点を画面上部側に変更
        val matrix = Matrix()
        matrix.preRotate(-90f, w / 2f, h / 2f) // グラデーションの開始位置を上部に設定
        gradient.setLocalMatrix(matrix)
        paint.shader = gradient
```
