## MotionToolBar実装時の動作について

・子のフラグメントのレイアウトに`NestedScrollView`または`RecyclerView`がある時は、Toolbarがそんざいする画面には`NestedScrollView`を入れる必要ない

例

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"

    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appbar"
        android:background="@drawable/card_back"
        android:layout_height="190dp"
        android:layout_width="match_parent">
<androidx.constraintlayout.motion.widget.MotionLayout
    android:layout_width="match_parent"
    android:id="@+id/motion"
    android:layout_weight="1"
    android:layout_height="match_parent"
    android:minHeight="70dp"
    app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
    app:layoutDescription="@xml/fragment_home_scene">
<ImageView

    android:scaleType="centerCrop"
    android:id="@+id/back"
    android:src="@drawable/tab_image"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
    <ImageView
        android:id="@+id/logo"
        android:src="@drawable/logo"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>
</androidx.constraintlayout.motion.widget.MotionLayout>

        <com.google.android.material.tabs.TabLayout
            android:id="@+id/tabs"
            android:background="@android:color/transparent"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <com.google.android.material.tabs.TabItem
                android:layout_height="wrap_content"
                android:layout_width="wrap_content"
                android:text="Ticket" />

            <com.google.android.material.tabs.TabItem
                android:layout_height="wrap_content"
                android:layout_width="wrap_content"
                android:text="Time Table" />

            <com.google.android.material.tabs.TabItem
                android:layout_height="wrap_content"
                android:layout_width="wrap_content"
                android:text="Map" />
        </com.google.android.material.tabs.TabLayout>
    </com.google.android.material.appbar.AppBarLayout>
  //NestedScrollViewではなくLinearLayoutを指定
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="com.google.android.material.appbar.AppBarLayout$ScrollingViewBehavior">

            <androidx.fragment.app.FragmentContainerView
                android:id="@+id/fragmentContainerView"
                android:name="com.example.metoro_map.TicketFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />

    </LinearLayout>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

・子のフラグメント

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    tools:context=".TicketFragment">
//こちらで使用
    <androidx.core.widget.NestedScrollView
        android:padding="10dp"
        android:layout_weight="1"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

        </LinearLayout>
    </androidx.core.widget.NestedScrollView>
</LinearLayout>
```

このようにすることで、画面によりスクロールの有無が変わる場合に、スクロールの必要がない画面ではAppBarの状態が変わらなくなる
