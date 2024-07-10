# アニメーションの基本

## 1.プロパティアニメーション

・アニメーターについて

`ValueAnimator`: よりコアなクラス、アニメーションに必要な機能は全て備える

`ObjectAnimator`: ターゲットのオブジェクトとプロパティを指定できるValueAnimatorの拡張版。基本これを使う

`AnimatorSet`: 複数のアニメーションをグループ化する

## ValueAnimatorの例

```kotlin
ValueAnimator.ofFloat(0f, 100f).apply {
    duration = 1000
    start()
}
```

・このコードの場合値が0fから100fまでの計算をアニメーションする

### Viewに値を適用する

`addUpdateListener`を使用する

```kotlin
 addUpdateListener { updatedAnimation ->
        textView.translationX = updatedAnimation.animatedValue as Float
    }
```

## ObjectAnimatorの例

```kotlin
ObjectAnimator.ofFloat(textView, "translationX", 100f).apply {
    duration = 1000
    start()
}
```

・アニメーションを適用するViewとプロパティを指定することで、リスナの設定などの必要がなくなる


## AnimationSetの例

```kotlin
val bouncer = AnimatorSet().apply {
    play(bounceAnim).before(squashAnim1)
    play(squashAnim1).with(squashAnim2)
    play(squashAnim1).with(stretchAnim1)
    play(squashAnim1).with(stretchAnim2)
    play(bounceBackAnim).after(stretchAnim2)
}
val fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f).apply {
    duration = 250
}
AnimatorSet().apply {
    play(bouncer).before(fadeAnim)
    start()
}
```

・このコードのアニメーションの順序
  1.bounceAnim
  
  2.squashAnim1,squashAnim2,stretchAnim1,stretchAnim2
  
  3.bounceBackAnim

  4.fadeAnim

  ## StateListAnimatorの使用

・StateListAnimatorを使用すると、Viewの状態変化をアニメーションできる

例)Butttonのpress時

・xmlを使用して`selector`で定義する

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- the pressed state; increase x and y size to 150% -->
    <item android:state_pressed="true">
        <set>
            <objectAnimator android:propertyName="scaleX"
                android:duration="@android:integer/config_shortAnimTime"
                android:valueTo="1.5"
                android:valueType="floatType"/>
            <objectAnimator android:propertyName="scaleY"
                android:duration="@android:integer/config_shortAnimTime"
                android:valueTo="1.5"
                android:valueType="floatType"/>
        </set>
    </item>
    <!-- the default, non-pressed state;-->
    <item android:state_pressed="false">
        <set>
            <objectAnimator android:propertyName="scaleX"
                android:duration="@android:integer/config_shortAnimTime"
                android:valueTo="1"
                android:valueType="floatType"/>
            <objectAnimator android:propertyName="scaleY"
                android:duration="@android:integer/config_shortAnimTime"
                android:valueTo="1"
                android:valueType="floatType"/>
        </set>
    </item>
</selector>
```

・Viewにアニメーションを接続する

```xml
<Button android:stateListAnimator="@xml/animate_scale"
        ... />
```

## 曲線モーションを追加する

・`Path`を使用することで、直線的でなく指定座標を経由するようなアニメーションを定義できる

```kotlin
    val path = Path().apply {
        arcTo(0f, 0f, 1000f, 1000f, 270f, -180f, true)
    }
    val animator = ObjectAnimator.ofFloat(view, View.X, View.Y, path).apply {
        duration = 2000
        start()
    }
}
```
