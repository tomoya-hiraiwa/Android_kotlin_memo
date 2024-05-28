# BarChart

## TextViewで作成する

### `backGround`に色を設定しておき、View幅でグラフのように見せる

例

```xml
 <TextView
        android:id="@+id/graph"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:background="@color/black" />
```

```kotlin
b.graph.layoutParams = LayoutParams(graphWidth, LayoutParams.WRAP_CONTENT) //graphWidth: グラフの長さ
```

### 増加アニメーションをつける場合

```kotlin
val scope = CoroutineScope(Dispatchers.IO)
            scope.launch {
                var width = 0
                while (width < max){
                    width += 10
                    withContext(Dispatchers.Main) {
                        b.graph.layoutParams = LayoutParams(width, LayoutParams.WRAP_CONTENT)
                    }
                    delay(20)
                }
            }
            b.graph.layoutParams  = LayoutParams(data.medal_point* 10,LayoutParams.WRAP_CONTENT)
}
```

## 縦向きのBarChart

### RecylcerViewにTextViewの入ったアイテムを表示する

実装例

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:padding="10dp"
    android:gravity="bottom"
    android:orientation="vertical">
<TextView
    android:id="@+id/bar"
    android:background="@color/black"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
    <TextView
        android:id="@+id/name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="aaa"/>
</LinearLayout>
```

>[!IMPORTANT]
> ### グラフを上向きに伸ばすために
>
>親レイアウトに`android:gravity="bottom"`をつけること

・RecyclerViewの設定

```kotlin
 val list = b.list
        val manager = LinearLayoutManager(this).apply {
            orientation = LinearLayoutManager.HORIZONTAL
        }
        list.layoutManager = manager
```

・アダプター

```kotlin
class ListAdapter(private val dataList: List<Status>): RecyclerView.Adapter<ListAdapter.ListViewHolder>() {
    inner class ListViewHolder(private val b: ListItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(data: Status){
            b.name.text = data.name
           //親のViewがLinearLayoutなので、LinearLayout.LayoutParamsを使用
            b.bar.layoutParams  = LinearLayout.LayoutParams(100,data.count*10)
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ListViewHolder {
        return ListViewHolder(ListItemBinding.inflate(LayoutInflater.from(parent.context),parent,false))
    }

    override fun getItemCount(): Int {
       return dataList.size
    }

    override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
        val data = dataList[position]
        holder.bindData(data)
    }
}
```

![bar](/photos/bar_vertical.png)


## Viewを自作する

### 棒グラフ部分のViewを準備

```kotlin
class BarGraph(context: Context,attrs: AttributeSet): View(context, attrs) {
    var value = 0f
    private val paint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        strokeWidth = 50f
        strokeCap = Paint.Cap.SQUARE
        style = Paint.Style.FILL
        color = Color.BLACK
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        canvas!!.drawLine(canvas.width /2f, canvas.height.toFloat(), canvas.width /2f,canvas.height - value * 10f,paint)
    }
}
```

### リストのアイテムとして組み込み

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="100dp"
    android:gravity="bottom|center_horizontal"
    android:layout_height="wrap_content">
  <com.example.sales_list.BarGraph
      android:id="@+id/bar"
      android:layout_width="20dp"
      android:layout_height="100dp"/>
    <com.google.android.material.divider.MaterialDivider
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
    <TextView
        android:id="@+id/name"
        android:layout_marginTop="10dp"
        android:textSize="20sp"
        android:textAlignment="center"
        android:text="aaa"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
</LinearLayout>
```

### リストアダプターで値を指定

```kotlin
class BarChartAdapter(private val dataList: List<PieceData>): RecyclerView.Adapter<BarChartAdapter.BarChartViewHolder>(){
    inner class BarChartViewHolder(private val b: BarGraphItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(data: PieceData){
            b.apply {
                name.text = data.name
                bar.value = data.piece.toFloat()
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): BarChartViewHolder {
        return BarChartViewHolder(BarGraphItemBinding.inflate(LayoutInflater.from(parent.context),parent,false))
    }

    override fun getItemCount(): Int {
        return dataList.size
    }

    override fun onBindViewHolder(holder: BarChartViewHolder, position: Int) {
        holder.bindData(data = dataList[position])
    }
}
```

### メリット

・Viewクラス作成時に必ず上に伸びるように指定しているので、関係部分のViewのGravityを気にする必要がない

・アニメーションをつける時に`value`の値を変えるだけで済むため、コードが短縮できる

![barchartCode](/photos/barchart_code.png)

