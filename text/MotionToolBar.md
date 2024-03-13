# MotionToolBar

## RelativeLayout + AppBarLayout + MotionLayout + NestedScrollViewで動きのあるToolBarを作成

![video](/video/motion_toolbar.gif)

## レイアウト準備

### root要素に`CordinatorLayout`を指定

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".HomeActivity">
```

### `AppBarLayout`を追加する

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".HomeActivity">
    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/bar"
        android:layout_marginTop="-30dp"
        android:layout_width="match_parent"
        android:layout_height="200dp">
  </com.google.android.material.appbar.AppBarLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

> [!IMPORTANT]
> `layout_height`属性に展開時のToolBarの高さを指定しておく

### `MotionLayout`を組み込む

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".HomeActivity">
    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/bar"
        android:layout_marginTop="-30dp"
        android:layout_width="match_parent"
        android:layout_height="200dp">
        <androidx.constraintlayout.motion.widget.MotionLayout
            android:layout_width="match_parent"
            android:id="@+id/motionLayout"
            android:layout_height="match_parent"
            android:background="@color/orange"
            android:minHeight="100dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
            app:layoutDescription="@xml/activity_home_scene">
            <!-- ここに動的にレイアウトを変更したいViewを定義する -->
        </androidx.constraintlayout.motion.widget.MotionLayout>
    </com.google.android.material.appbar.AppBarLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

> [!IMPORTANT]
> `layout_scrollFlags`属性を設定すること

### `NestedScrollView`を追加

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".HomeActivity">
    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/bar"
        android:layout_marginTop="-30dp"
        android:layout_width="match_parent"
        android:layout_height="200dp">
        <androidx.constraintlayout.motion.widget.MotionLayout
            android:layout_width="match_parent"
            android:id="@+id/motionLayout"
            android:layout_height="match_parent"
            android:background="@color/orange"
            android:minHeight="100dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
            app:layoutDescription="@xml/activity_home_scene">
          <!-- ここに動的にレイアウトを変更したいViewを定義する -->
        </androidx.constraintlayout.motion.widget.MotionLayout>
    </com.google.android.material.appbar.AppBarLayout>
    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        android:layout_height="match_parent">
        <LinearLayout
            android:padding="10dp"
            android:layout_marginTop="50dp"
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="match_parent">
            <include layout="@layout/card_circle_graph"
                android:id="@+id/circle"/>
        </LinearLayout>
    </androidx.core.widget.NestedScrollView>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

> [!IMPORTANT]
> `layout_behavior`属性を`@string/appbar_scrolling_view_behavior`で設定する

### MotionScene用のxmlファイルでUIの移動を定義

・詳細は[MotionLayout](/text/MotionLayout.md)参照

### motionLayoutとAppBarLayoutの動作を同期さえるコードを記述

```kotlin
 b.bar.addOnOffsetChangedListener(
            AppBarLayout.OnOffsetChangedListener { appBarLayout, verticalOffset ->
                val seekPosition = -verticalOffset / appBarLayout.totalScrollRange.toFloat()
                b.motionLayout.progress = seekPosition
            }
        )
```

