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
