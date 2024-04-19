# AppBar

## TopAppBar

・サイズによって4種類のアプリバーに分けられる

|タイプ|概要|
|:--:|:--:|
|小|タイトル表示など、動作はあまり実装しないもの|
|中央揃え|タイトルを中央に表示する|
|中|ある程度のナビゲーション機能や動作を行う時向け|
|大|より多くの機能を実装する場合|

### 主要パラメータ

・`title`:AppBarのタイトル
・`navigationIcon`:AppBar左側に表示するドロワー展開用などのアイコン
・`scrollBehavior`:画面スクロールに対し、AppBarの挙動を定義
・`colors`:色の設定

### TopBar(小)

```kotlin
@Composable
fun SmallTopBar(){
    Scaffold(topBar = {
        TopAppBar(title = { Text(text = "SmallTopBar") },
            colors = TopAppBarDefaults.topAppBarColors(
                containerColor = MaterialTheme.colorScheme.primaryContainer,
                titleContentColor = MaterialTheme.colorScheme.primary
            ))
    }) {it ->
        Column(modifier = Modifier.padding(it)) {

        }
    }
}
```

![smalltopbar](/photos/smalltopbar.png)

### AppBar(中央)

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun CenterTopBar(){
    val scrollBehavior  = TopAppBarDefaults.pinnedScrollBehavior(rememberTopAppBarState())
    Scaffold(
        modifier = Modifier.nestedScroll(scrollBehavior.nestedScrollConnection),
        topBar = {
            CenterAlignedTopAppBar(title = { Text(text = "CenterTopBar")},
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                ),
                navigationIcon = {
                    Icon(imageVector = Icons.Filled.ArrowBack, contentDescription = "back")
                },
                actions = {
                    IconButton(onClick = {}) {
                        Icon(imageVector = Icons.Filled.Menu, contentDescription = "menu")
                    }
                },
                scrollBehavior = scrollBehavior
            )
        }
    ) {innerPadding ->
      LazyColumn(modifier = Modifier.padding(innerPadding)){
          val sampleData = List(100){"$it"}
          items(sampleData){item ->
              SampleListItem(text = item)
          }
      }
    }
}
```

![centertopbar](/photos/centertopbar.png)

### AppBar(Medium)

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MediumTopBar(){
    val scrollBehavior = TopAppBarDefaults.enterAlwaysScrollBehavior(rememberTopAppBarState())
    Scaffold(
        modifier = Modifier.nestedScroll(scrollBehavior.nestedScrollConnection),
        topBar = {
            MediumTopAppBar(title = {
                Text(text = "Medium Top AppBar")
            },
                colors = TopAppBarDefaults.mediumTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                ),
                navigationIcon = {Icon(imageVector = Icons.Filled.ArrowBack,"back")},
                actions = {IconButton(onClick = {  }) {
                    Icon(imageVector = Icons.Filled.Menu, contentDescription = "menu")
                } },
                scrollBehavior = scrollBehavior)
        }
    ) {innerPadding ->
        LazyColumn(modifier = Modifier.padding(innerPadding)){
            val sampleData = List(100){"$it"}
            items(sampleData){item ->
                SampleListItem(text = item)
            }
        }
    }
}
```

![mediumtopbar](/photos/mediumtopbar1.png)

![mediumtopbar2](/photos/mediumtopbar2.png)

### TopAppBar(大)

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun LargeTopBar(){
    val scrollBehavior = TopAppBarDefaults.enterAlwaysScrollBehavior(rememberTopAppBarState())
    Scaffold(
        modifier = Modifier.nestedScroll(scrollBehavior.nestedScrollConnection),
        topBar = {
            LargeTopAppBar(
                title = {
                    Text(text = "Large Top AppBar")
                },
                colors = TopAppBarDefaults.largeTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                ),
                navigationIcon = {
                    Icon(imageVector = Icons.Filled.ArrowBack, contentDescription = "back")
                },
                actions = {
                    IconButton(onClick = { /*TODO*/ }) {
                        Icon(imageVector = Icons.Filled.Menu, contentDescription = "menu")
                    }
                },
                scrollBehavior = scrollBehavior
            )
        }
    ) {innerPadding ->
        LazyColumn(modifier = Modifier.padding(innerPadding)){
            val sampleData = List(100){"$it"}
            items(sampleData){item ->
                SampleListItem(text = item)
            }
        }
    }
```

![largetopbar](/photos/largetopbar1.png)

![largetopbar2](/photos/largetopbar2.png)

## BottomAppBar

```kotlin
@Composable
fun BottomAppBarSample(){
    Scaffold(
        bottomBar = {
            BottomAppBar(actions ={
                IconButton(onClick = { /*TODO*/ }) {
                    Icon(imageVector = Icons.Filled.Check, contentDescription = "check")
                }
                IconButton(onClick = { /*TODO*/ }) {
                    Icon(imageVector = Icons.Filled.Edit, contentDescription = "edit")
                }
                IconButton(onClick = { /*TODO*/ }) {
                    Icon(imageVector = Icons.Filled.Email, contentDescription = "mail")
                }
                IconButton(onClick = { /*TODO*/ }) {
                    Icon(imageVector = Icons.Filled.AccountCircle, contentDescription = "account")
                }
            },
                floatingActionButton = {
                    FloatingActionButton(onClick = { /*TODO*/ }) {
                        Icon(imageVector = Icons.Filled.Add, contentDescription = "add")
                    }
                })
        }
    ) {innerPadding ->
        LazyColumn(modifier = Modifier.padding(innerPadding)){
            val sampleData = List(100){"$it"}
            items(sampleData){item ->
                SampleListItem(text = item)
            }
        }

    }
}
```

![bottomappbar](/photos/bottomAppBar.png)
