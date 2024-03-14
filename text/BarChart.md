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
