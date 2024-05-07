# ColorPicker

## 1.一色分のレイアウトを作る

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <FrameLayout
        android:id="@+id/select"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:background="#FF0000"
        android:padding="5dp">

        <FrameLayout
            android:id="@+id/color_container"
            android:layout_width="match_parent"
            android:layout_height="match_parent"></FrameLayout>
    </FrameLayout>


</FrameLayout>
```

## 2.表示したい画面にコンテナを準備

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="10dp"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="ColorPicker"
        android:textSize="20sp"
        android:textStyle="bold" />

    <LinearLayout
        android:id="@+id/picker_container"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal" />

</LinearLayout>
```

## 3.色の数分のViewを生成するメソッド作成

```kotlin
    private fun drawPicker(){
        //delete all views from container
        b.pickerContainer.removeAllViews()
        for (i in colorList.indices){
            //get color
            val color = colorList[i]
            //inflate color item layout
            val cb = ColorItemBinding.inflate(layoutInflater)
            cb.apply {
                if (color == selectColor){
                    select.setBackgroundColor(Color.RED)
                }
                else select.setBackgroundColor(Color.TRANSPARENT)
                colorContainer.setBackgroundColor(ContextCompat.getColor(this@MainActivity,color))
                root.setOnClickListener {
                    selectColor = color
                    b.root.setBackgroundColor(ContextCompat.getColor(this@MainActivity,selectColor))
                    //call myself
                    drawPicker()
                }
            }
            //add this views to container
            b.pickerContainer.addView(cb.root)
        }
    }
```
