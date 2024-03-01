# ContainerTransition

## Fragment内RecyclerViewから別Framgnetへのトランジション

1.RecyclerView.Adapterのクリックリスナーで遷移元のViewを渡す

例：
```kotlin
class WordListAdapter(private val dataList: MutableList<Word>): RecyclerView.Adapter<WordListAdapter.WordListViewHolder>() {
    private var wordListClicker: WordListClicker? = null
    interface WordListClicker{
        fun onClick(data: Word,view: View)
    }
    fun setOnWordListClicker(lis: WordListClicker){
        wordListClicker = lis
    }
    inner class WordListViewHolder(private val b: WordListItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(pos: Int){
            val data = dataList[pos]
            b.word.text = data.word
            b.mean.text = "訳：${data.mean}"
            b.root.setOnClickListener {
                //Viewにトランジションの名前を渡す
                b.root.transitionName = "to_detail"
                //Viewの要素も渡す
                wordListClicker?.onClick(data,b.root)
            }
        }
    }
```

2.遷移元(RecyclerViewがある)Fragmentで画面遷移をさせる


```kotlin
   adapter.setOnWordListClicker(object: WordListAdapter.WordListClicker{
                override fun onClick(data: Word,view: View) {
                    parentFragmentManager.beginTransaction()
                        //リサイクラービューのアイテムにつけたものと同じ名前をつける
                        .addSharedElement(view,"to_detail")
                        .replace(R.id.fragmentContainerView,VocDetailFragment())
                        .commit()
                }
            })
```

3.遷移先のFragmentでMaterialContainerTransformを設定

```kotlin
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        sharedElementEnterTransition = MaterialContainerTransform()
    }
```

・遷移後の画面はbackgroundを設定すると綺麗に見える

例（テーマも考慮してコードで指定する）

```kotlin
val conf = requireActivity().resources.configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
        if (conf == Configuration.UI_MODE_NIGHT_YES){
            b.root.setBackgroundColor(requireContext().resources.getColor(R.color.md_theme_dark_background,requireActivity().theme))
        }
        else b.root.setBackgroundColor(requireContext().resources.getColor(R.color.md_theme_light_background,requireActivity().theme))
```

