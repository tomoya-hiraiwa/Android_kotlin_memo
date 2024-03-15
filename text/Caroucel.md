# Caroucel

## 無限カルーセルの作成

・`ViewPager2` + `RecyclerView.Adapter`で作成

#### Adapter

例

```kotlin
class ListAdapter: RecyclerView.Adapter<ListAdapter.ListViewHolder>(){
//実際のデータの個数+2サイズの枠を準備
     val addCount = dataList.size +2
    inner class ListViewHolder(private val b: CaroucelItemBinding): RecyclerView.ViewHolder(b.root){
        fun bindData(data:  String){
            b.textView.text = data
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ListViewHolder {
        return ListViewHolder(CaroucelItemBinding.inflate(LayoutInflater.from(parent.context),parent,false))
    }

    override fun getItemCount(): Int {
    //用意した枠数の変数を渡す
        return addCount
    }

    override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
        //実際に表示するデータのポジションを取得する
        val realPosition = when(position){
            0 -> dataList.size -1
            dataList.size +1 -> 0
            else -> position -1
        }
        val data = dataList[realPosition]
        holder.bindData(data)

    }
}
```

### View

```kotlin
//実際のポジションを保持する変数を定義しておく
private var nowPosition = 1
```

・アダプターをセット+ループするように設定

```kotlin
 val list = b.list
val adapter = ListAdapter()
list.currentItem = nowPosition
list.registerOnPageChangeCallback(object : ViewPager2.OnPageChangeCallback(){
            override fun onPageSelected(position: Int) {
                nowPosition = position

            }

            override fun onPageScrollStateChanged(state: Int) {
                if (state == ViewPager2.SCROLL_STATE_IDLE){
                    if (nowPosition == 0){
                        list.setCurrentItem(adapter.itemCount -2,false)
                        nowPosition  = adapter.addCount -2
                    }
                    else  if(nowPosition == adapter.itemCount -1 ){
                        list.setCurrentItem(1,false)
                        nowPosition  = 1
                    }
                }
            }
        })
```

## 左右のViewを表示

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp"
    android:foreground="?attr/selectableItemBackground"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/carousel_container"
    android:layout_marginStart="60dp"
    android:background="#F8BBD0"
    android:layout_marginEnd="60dp"
    app:shapeAppearance="?attr/shapeAppearanceCornerExtraLarge"
    android:orientation="vertical">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@+id/textView"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            android:text="aaatext" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</FrameLayout>
```

・両サイドにoffset + margin分の余白をつけておく

```kotlin
list.offscreenPageLimit = 2
 list.setPageTransformer {page,position ->
           page .translationX = -position * (3* margin + offset)
          //両サイドのViewのサイズ変更
           val scale = 1 -(abs(position)* 0.1f)
            page.scaleX = scale
            page.scaleY = scale
          //両サイドのViewの透明度変更
            page.alpha = 1 - (abs(position)* 0.7f)
        }
```

・数字の部分や`margin`,`offset`部分は適宜調整する
