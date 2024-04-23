# ピンチ、ドラッグ機能付きImageView

## 1.レイアウト

・`FrameLayout`で`ImageView`を囲む

```xml
  <FrameLayout
        android:background="@color/black"
        android:id="@+id/frame"
        android:layout_marginTop="50dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <ImageView
            android:adjustViewBounds="true"
            android:id="@+id/map"
            android:background="@color/black"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:src="@drawable/train" />
    </FrameLayout>
```

こうして、frameLayoutのリスナを使用することで、サイズ変更や移動時のカクツキを無くすことができる

## 2.Scale変更

・現在のサイズを保持する変数を設定する

```kotlin
    private var normalFactor = 1.0f //初期値は1倍
  ```

・スケール変更する変数を定義

```kotlin
 val scaleGestureDetector =
            ScaleGestureDetector(requireContext(), object : OnScaleGestureListener {
                override fun onScale(detector: ScaleGestureDetector): Boolean {
                    normalFactor *= detector.scaleFactor
                    normalFactor = Math.max(1.0f, Math.min(normalFactor, 3.0f)) //1.0f：最小倍率、3.0f:最大倍率
                    b.map.scaleX = normalFactor
                    b.map.scaleY = normalFactor
                    return true
                }

                override fun onScaleBegin(detector: ScaleGestureDetector): Boolean {
                    return true // Indicate that we're handling scaling
                }

                override fun onScaleEnd(detector: ScaleGestureDetector) {
                }
            })
```

## 3.Drag動作

```kotlin
val dragDetector =
            GestureDetector(requireContext(), object : GestureDetector.SimpleOnGestureListener() {
                override fun onScroll(
                    e1: MotionEvent,
                    e2: MotionEvent,
                    distanceX: Float,
                    distanceY: Float
                ): Boolean {
                    val valueX = b.map.translationX - distanceX
                    val valueY = b.map.translationY - distanceY
                    var transitionX = Math.max(
                        0f,
                        Math.min(
                            Math.abs(valueX),
                            Math.abs(b.frame.width * normalFactor - b.map.width.toFloat())
                        )
                    )
                    var transitionY = Math.max(
                        0f,
                        Math.min(
                            Math.abs(valueY),
                            Math.abs(b.frame.height * normalFactor - b.map.height.toFloat())
                        )
                    )
                    if (valueX < 0f) transitionX *= -1f
                    if (valueY < 0f) transitionY *= -1f
                    b.map.translationX = transitionX
                    b.map.translationY = transitionY
                    return true
                }
            })
```

・どの値を使用するかのときは一度絶対値を用い、その後再び符号を戻す作業をする

・最大幅はframeLayoutのサイズに現在の倍率をかけ、imageViewのサイズを引いたサイズをドラッグ範囲として指定

## 4.リスナとイベントんの設定

```kotlin
//FrameLayoutのリスナを使用する
   b.frame.setOnTouchListener { v, event ->
            scaleGestureDetector.onTouchEvent(event)
            dragDetector.onTouchEvent(event)
            true
        }
```
