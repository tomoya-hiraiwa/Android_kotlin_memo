# MainActivityのRecyclerView→DetailActivivtyへの遷移アニメーション

<details>

<summary>動作例</summary>

![sample](video/main_to_detail.gif)

</details>

## 実装方針

1.各画面のレイアウトおよび、リストアイテムのレイアウトを作成（省略）

2.共有要素としてアニメーションしたいViewに二つのアクティビティ間で共有のTransitionNameをつける

3.startActivity時にActivirtOptionsとして情報を渡す

<details>
  <summary>データ用のItemクラス</summary>

  ```kotlin
data class Item(
    val name: String,
    val author: String,
    val fileName: String
) {

    companion object {
        private const val LARGE_BASE_URL = "https://storage.googleapis.com/androiddevelopers/sample_data/activity_transition/large/"
        private const val THUMB_BASE_URL = "https://storage.googleapis.com/androiddevelopers/sample_data/activity_transition/thumbs/"

        val ITEMS = arrayOf(
            Item("Flying in the Light", "Romain Guy", "flying_in_the_light.jpg"),
            Item("Caterpillar", "Romain Guy", "caterpillar.jpg"),
            Item("Look Me in the Eye", "Romain Guy", "look_me_in_the_eye.jpg"),
            Item("Flamingo", "Romain Guy", "flamingo.jpg"),
            Item("Rainbow", "Romain Guy", "rainbow.jpg"),
            Item("Over there", "Romain Guy", "over_there.jpg"),
            Item("Jelly Fish 2", "Romain Guy", "jelly_fish_2.jpg"),
            Item("Lone Pine Sunset", "Romain Guy", "lone_pine_sunset.jpg"),
        )

        fun getItem(id: Int): Item? = ITEMS[id]
        fun getDetail(id: Int): Item? = ITEMS.firstOrNull { it.id  == id }
    }

    val id: Int = name.hashCode() + fileName.hashCode()

    val photoUrl: String = LARGE_BASE_URL + fileName

    val thumbnailUrl: String = THUMB_BASE_URL + fileName
}
```

</details>

・ListAdapter

```kotlin
class ListAdapter(): RecyclerView.Adapter<ListAdapter.ListViewHolder>(){
    private var listClicker: ListClicker? = null
    interface ListClicker{
        //画像部分とタイトル部分のViewを共有するために引数で渡す
        fun onClick(data: Item,imageView: View,nameView: View)
    }
    fun setOnListClicker(lis: ListClicker){
        listClicker = lis
    }
    inner class ListViewHolder(private val b: GridItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(data: Item){
            Glide.with(b.root)
                .load(data.thumbnailUrl)
                .into(b.imageviewItem)
            b.textviewName.text = data.name
            b.root.setOnClickListener {
                listClicker?.onClick(data, b.imageviewItem,b.textviewName)
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ListViewHolder {
        return ListViewHolder(inflate(LayoutInflater.from(parent.context),parent,false))
    }

    override fun getItemCount(): Int {
        return Item.ITEMS.size
    }

    override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
        val item = Item.getItem(position)
        holder.bindData(item!!)
    }
}
```

・MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var b: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        val list = b.gridList
        list.layoutManager = GridLayoutManager(this,2)
        val adapter = ListAdapter()
        list.adapter = adapter
        adapter.setOnListClicker(object : ListAdapter.ListClicker{
            override fun onClick(data: Item, view: View, nameView: View) {
                val intent  = Intent(this@MainActivity,DetailsActivity::class.java).apply {
                    //詳細画面でのデータ取得ようにIDを渡す
                    putExtra(DetailsActivity.EXTRA_PARAM_ID,data.id)
                }
                //共有要素の設定
                val activityOptions = ActivityOptionsCompat.makeSceneTransitionAnimation(
                    this@MainActivity,
                    //共有したいView,共有の名前のPair型で定義
                    Pair(view,DetailsActivity.VIEW_NAME_HEADER_IMAGE),
                    Pair(nameView,DetailsActivity.VIEW_NAME_HEADER_TITLE)
                )
                //作成したActivityOptionsをバンドルとして渡す　
                startActivity(intent,activityOptions.toBundle())
            }
        })
    }
}
```

・DetailActivity.kt

```kotlin
class DetailsActivity : AppCompatActivity() {
    companion object{
        const val EXTRA_PARAM_ID = "detail:_id"
        const val VIEW_NAME_HEADER_IMAGE = "detail:header:image"
        const val VIEW_NAME_HEADER_TITLE = "detail:header:title"
    }
    private lateinit var mHeaderIMageView: ImageView
    private lateinit var mHeaderTitle: TextView
    private lateinit var mItem: Item
    private lateinit var b: ActivityDetailsBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        b = ActivityDetailsBinding.inflate(layoutInflater)
        setContentView(b.root)
        //データの取得
        mItem = Item.getDetail(intent.getIntExtra(EXTRA_PARAM_ID,0))?: return
        mHeaderIMageView = b.imageviewHeader
        mHeaderTitle = b.textviewTitle
        //Detail画面側の共有要素Viewに共通の名前をつける
        ViewCompat.setTransitionName(mHeaderIMageView, VIEW_NAME_HEADER_IMAGE)
        ViewCompat.setTransitionName(mHeaderTitle, VIEW_NAME_HEADER_TITLE)

        loadItem()
    }
    //データ呼び出しメソッド
    private fun loadItem(){
        mHeaderTitle.text = getString(R.string.image_header,mItem.name,mItem.author)
        Glide.with(mHeaderIMageView.context)
            .load(mItem.thumbnailUrl)
            .into(mHeaderIMageView)
    }
}
```

<details>
  <summary>参考：正方形にするカスタムFrameLayout</summary>

  ```kotlin
class SquareFrameLayout(context: Context,attrs: AttributeSet): FrameLayout(context,attrs) {
    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        val widthSize = MeasureSpec.getSize(widthMeasureSpec)
        val heightSize = MeasureSpec.getSize(heightMeasureSpec)

        val size = when{
            //どちらの値も指定されていない場合
            widthSize == 0 && heightSize == 0 ->{
                //親クラスサイズをそのまま使用
                super.onMeasure(widthMeasureSpec, heightMeasureSpec)
                measuredWidth.coerceAtMost(measuredHeight)
            }
            //どちらかだけ指定されいてる場合→小さい方の値を使用
            widthSize == 0 || heightSize == 0 -> widthSize.coerceAtLeast(heightSize)
                //どちらも指定されている場合→小さい方の値を使用
            else -> widthSize.coerceAtMost(heightSize)
        }
        //カスタムしたサイズを使用して新しく渡す値を作成
        val newMeasureSpec = MeasureSpec.makeMeasureSpec(size,MeasureSpec.EXACTLY)
        //親のメソッドに新しい値を引数として渡して呼び出す
        super.onMeasure(newMeasureSpec, newMeasureSpec)
    }
}
```
</details>
