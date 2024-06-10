## Activitiy→Activityの画面遷移

・基本コード(遷移もとActivity内)

```kotlin
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        //アニメーションを許可
        window.requestFeature(Window.FEATURE_CONTENT_TRANSITIONS)
        //アニメーションのタイプを設定
        window.exitTransition = Explode()
        setContentView(binding.root)
        val list = binding.list
        list.layoutManager = GridLayoutManager(this,2)
        val adapter = ListAdapter(dataList)
        list.adapter = adapter
        adapter.setOnListener(object:ListAdapter.ListClicker{
            override fun onClick(data: Food) {
                startActivity(Intent(this@MainActivity,DetailActivity::class.java).apply {
                    putExtra("name",data.name)
                    putExtra("image",data.image)
                },
                //アニメーションを設定
                ActivityOptions.makeSceneTransitionAnimation(this@MainActivity).toBundle())
            }
        })
    }
}
```

・Explodeの場合

![explode](https://github.com/tomoya-hiraiwa/Android_kotlin_memo/blob/main/video/explode.gif)

・Slideの場合 Slide(Gravity.~)でスライド方向設定可能

![slide](https://github.com/tomoya-hiraiwa/Android_kotlin_memo/blob/main/video/Slide.gif)

<br>

## 2024/06/10 追記

### MainActivity→新しいアクティビティ

・各トランジションプロパティの挙動

`exitTransition`: アクティビティが別のアクティビティに変わる時に作動する

今回の場合であれば、main→newの場合のmainに定義

`reenterTransition`: トランジション先から元のViewに戻ってきた時の動作

今回であればnew側でfinish→mainのmain側の戻ってきた時の挙動

`enterTransition`: 画面が呼ばれた時の動作

今回であればmain→newのnew側

`returnTransition`: `finishAfterTransition`でアクティビティを終了する時の動作

今回であればnew側finishの時の動作

コード例

・main

```kotlin
  window.reenterTransition = Slide(Gravity.START).apply {
            duration =1000
            addTarget(R.id.parent_main)
        }
       //ライフサイクルの関係か、durationなどを遅くするとアニメーションつかなくなる
        window.exitTransition = Slide(Gravity.START)

        super.onCreate(savedInstanceState)
        b = ActivityMainBinding.inflate(layoutInflater)
        setContentView(b.root)
        b.apply {
            startButton.setOnClickListener {
                val bundle = ActivityOptions.makeSceneTransitionAnimation(this@MainActivity).toBundle()
                startActivity(Intent(this@MainActivity,ListActivity::class.java),bundle)
            }
        }

    }
```

・new側

```kotlin
window.enterTransition = Slide(Gravity.END).apply {
            duration = 1000
            excludeTarget(android.R.id.statusBarBackground, true)
            excludeTarget(android.R.id.navigationBarBackground, true)
        }
        window.returnTransition = Slide(Gravity.END).apply {
            duration = 200
            interpolator = LinearInterpolator()
            excludeTarget(android.R.id.statusBarBackground, true)
            excludeTarget(android.R.id.navigationBarBackground, true)
        }
        window.allowEnterTransitionOverlap = true

        super.onCreate(savedInstanceState)
        b = ActivityListBinding.inflate(layoutInflater)
        setContentView(b.root)
    }

    override fun onBackPressed() {
        super.onBackPressed()
        finishAfterTransition()
    }
}
```
