# MotionLayout

## 使用方法

### レイアウト定義

例 activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.motion.widget.MotionLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layoutDescription="@xml/scene_01"
    tools:context=".MainActivity">

   <View
       android:id="@+id/button"
       android:background="@color/black"
       android:layout_width="100dp"
       android:layout_height="100dp"/>

</androidx.constraintlayout.motion.widget.MotionLayout>
```

・`layoutDescription`タグにモーションシーンのxmlファイルを指定する

### モーションシーンファイルの作成

例 scene_01.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<MotionScene xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:motion="http://schemas.android.com/apk/res-auto">
    <Transition motion:constraintSetStart="@+id/start"
            motion:constraintSetEnd="@+id/end"
        motion:duration="1000">
        <OnSwipe motion:touchAnchorId="@+id/button"
            motion:touchAnchorSide="right"
            motion:dragDirection="dragRight"/>
    </Transition>
    <ConstraintSet android:id="@+id/start">
        <Constraint android:id="@+id/button"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_marginStart="8dp"
            motion:layout_constraintBottom_toBottomOf="parent"
            motion:layout_constraintTop_toTopOf="parent"
            motion:layout_constraintStart_toStartOf="parent"/>
    </ConstraintSet>
    <ConstraintSet android:id="@+id/end">
        <Constraint android:id="@+id/button"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_marginEnd="8dp"
            motion:layout_constraintBottom_toBottomOf="parent"
            motion:layout_constraintTop_toTopOf="parent"
            motion:layout_constraintEnd_toEndOf="parent"/>
    </ConstraintSet>
</MotionScene>
```

・`<Transition>`タグ:モーションの基本定義をする

  ・`motion:constraintSetStart`、`motion:constraintSetEnd`:モーションの始点、終点を定義

・`<OnSwipe>`タグ：モーションのタップコントロール

  ・`motion:touchAnchorId`:スワイプ、ドラッグするViewを指定

  ・`motion:touchAnchorSide`:アニメーション開始の動作方向（指定しなくてもいい可能性）

  ・`motion:dragDiretion`:操作方向

・`<ConstraintSet>`タグ:アニメーションの始点、終点のレイアウトを定義する

・`<Constraint>`タグ:レイアウト内の要素の位置や属性を定義する

## 実装サンプルなど

TBD
  
