## RecyclerView

### 使用方法

1.レイアウトに定義

2.レイアウトマネージャー設定

例：

```kotlin
list.layoutManager = LinearLayoutManager(this)
```

または

```kotlin
list.layoutManager = GridLayoutManager(this, 2)
```

・LinearLayoutManager

→リストを縦または横向きに一列づつ配置する

第1引数：Context

または

第1引数：Context,第2引数：リストの表示方向(Vertical or Horizonal),第3引数：表示データ逆順(Boolean)

・GridLayoutManager

→指定個数を同じ列or行に並べて配置する

第1引数：Context,第2引数：並べるリストアイテムの数(Int)

3.アダプターを設定

・RecyclerView.Adapterを継承したクラスを使用

例：

```kotlin
class ListAdapter(private val dataList: MutableList<Sample>): RecyclerView.Adapter<ListAdapter.ListViewHolder>() {
    //データを各Viewに当てはめるViewHolderクラス
    inner class ListViewHolder(private val b: ListItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(pos: Int){
            val data = dataList[pos]
            b.listTitle.text  = data.title
            b.listName.text = data.name
        }
    }
    //リストの各アイテムに使うレイアウトを指定
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ListViewHolder {
        return  ListViewHolder(ListItemBinding.inflate(LayoutInflater.from(parent.context),parent,false))
    }
    //アイテムの総数を指定
    override fun getItemCount(): Int {
        return dataList.size
    }
    //データを実際にリストに当てはめる
    override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
        holder.bindData(position)
    }
}
```

## リストのドラッグ動作

・ドラッグでアイテムの位置を入れ替える

・ItemTouchHelper.SimpleCallBackを継承したクラスを作成し、リストに当てはめる

例：

・ListTouchHelperクラス

```kotlin
inner class ListTouchHelper(): ItemTouchHelper.SimpleCallback(
        ItemTouchHelper.UP or ItemTouchHelper.DOWN, //第1引数：ドラッグ動作の方向を指定
        ItemTouchHelper.ACTION_STATE_IDLE          //第2引数：スワイプ動作の方向を指定
    ) {
        //ドラッグ時の動作を指定
        override fun onMove(
            recyclerView: RecyclerView,
            viewHolder: RecyclerView.ViewHolder,
            target: RecyclerView.ViewHolder
        ): Boolean {
            val data = dataList.removeAt(viewHolder.adapterPosition) //①
            dataList.add(target.adapterPosition,data)　//②
            adapter.notifyItemMoved(viewHolder.adapterPosition,target.adapterPosition)  //③
            return  true
        }
        //スワイプ時の動作を指定
        override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
        }

    }
```

・やっていること

①ドラッグ元のポジションのリストデータを削除プラス内容取得

②移動先の位置に①のデータ追加

③アダプターを更新

・RecyclerView取り付け

```kotlin
val listTouch = ItemTouchHelper(ListTouchHelper())
        listTouch.attachToRecyclerView(list)
```

## リストのスワイプ動作

・onSwipedメソッドに記述する

例：

```kotlin
override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
                dataList.removeAt(viewHolder.adapterPosition)
                adapter.notifyItemRemoved(viewHolder.adapterPosition)
        }
```

## リストスワイプ時の背景、アイコンの設定

・ItemTouchHelper.SimpleCallBack.onChildDrawメソッドをoverride

例：

```kotlin
override fun onChildDraw(
            c: Canvas,
            recyclerView: RecyclerView,
            viewHolder: RecyclerView.ViewHolder,
            dX: Float,
            dY: Float,
            actionState: Int,
            isCurrentlyActive: Boolean
        ) {
            super.onChildDraw(c, recyclerView, viewHolder, dX, dY, actionState, isCurrentlyActive)
            val background = ColorDrawable(Color.RED)
            val itemView = viewHolder.itemView
            val drawable = ResourcesCompat.getDrawable(resources, R.drawable.baseline_delete_24, theme)!!
            val iconMargin = (itemView.height - drawable.intrinsicHeight) / 2
            val iconTop = itemView.top + (itemView.height - drawable.intrinsicHeight) /2
            println(drawable)
            drawable.setBounds(itemView.left +iconMargin, itemView.top+iconMargin, itemView.left + drawable.intrinsicWidth + iconMargin, iconTop + drawable.intrinsicHeight)
            background.setBounds(itemView.left +10,itemView.top + 10, itemView.left + dX.toInt(), itemView.bottom -10)
            if (dX == 0f){
                background.setBounds(0,0,0,0)
            }
            background.draw(c)
            drawable.draw(c)
        }
```

・iconMargin→アイテム全体の高さからアイコンの高さをひいて半分することで、アイコンを垂直方向の中央に配置している（水平方向もこの値を使えば同じ余白）

・iconTop→アイテムビューの上端位置にアイコンの高さの半分を加算することで、アイコンがアイテムビューの上端から中央に配置されるようにしている

・setBoundsで要素の配置位置を決定(left,top,right,bottom)

・if(dX == 0f){...}→スワイプで位置が戻された時に背景をもう一度隠すため





