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

・Explodeの場合

