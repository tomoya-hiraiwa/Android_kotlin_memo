## ImageView

## Web上の画像を表示する方法

・基本使用例

```kotlin
var bitmap: Bitmap? = null
lifecycleScope.launch{
  withContext(Dispatchers.IO){
    val url = URL("http://...")
    val input = url.openStream()
    bitmap = BitmapFactory.decodeStream(input)
  }
  withContext(Dispatchers.Main){
    b.imageView.setIMageBitmap(bitmap)
  }
}
```

・画像データの取得部分はメインスレッドで行うとエラーが出るので、非同期、スレッド分けして行う

・リストへの表示例

```kotlin
class ListAdapter(private val dataList: List<News>): RecyclerView.Adapter<ListAdapter.ListViewHolder>(){
    inner class ListViewHolder(private val b: ListItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(pos: Int){
            val data = dataList[pos]
            var bitmap: Bitmap? = null
            b.listTitle.text = data.title
            val date = Date(data.date)
            val format = SimpleDateFormat("yyyy/MM/dd")
            b.listDate.text = format.format(date)
            val scope = CoroutineScope(Dispatchers.IO)
            scope.launch {
                val url = URL(data.photo_URL)
                val input = url.openStream()
                 bitmap = BitmapFactory.decodeStream(input)
                input.close()
                withContext(Dispatchers.Main){
                    b.listImage.setImageBitmap(bitmap)
                }
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ListViewHolder {
        return ListViewHolder(ListItemBinding.inflate(LayoutInflater.from(parent.context),parent,false))
    }

    override fun getItemCount(): Int {
        return dataList.size
    }

    override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
        holder.bindData(position)
    }
}
```

## `Canvas: trying to draw too large(~bytes) bitmap.`の対処

・このエラーはImageViewなどに表示する画像のファイルサイズが大きすぎる時に発生する

対処法

例:Bitmapを作成する際に、画像のサイズを縮小するオプションを追加する

```kotlin
val options = BitmapFactory.Options()
                options.inSampleSize = 2 //画像サイズを1/2にする
                val bitmap = BitmapFactory.decodeByteArray(decodeByte,0,decodeByte.size,options)
                b.imageView.setImageBitmap(bitmap)
```
