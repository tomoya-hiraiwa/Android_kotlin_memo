# Scaffold

・画面全体の基礎となるもの(足場という意味がある)

## 概要

・パラメータ3つある

1.`topBar`:アプリのトップに表示するToolBar

2.`bottomBar`:アプリの下端に配置するTooBar,Tab

3.`floatingActionButton`:FAB

実装例

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun ScaffoldSample(){
    var count by remember {
        mutableStateOf(0)
    }
    Scaffold(
        topBar = {
            TopAppBar(title = { Text(text = "Scaffold Sample")},
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                )
            )
        },
        bottomBar = {
            BottomAppBar(containerColor = MaterialTheme.colorScheme.primaryContainer,
                contentColor = MaterialTheme.colorScheme.primary){
                Text(text = "Bottom Bar",
                    modifier = Modifier.fillMaxWidth(),
                    textAlign = TextAlign.Center)
            }
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { count++ }) {
                Icon(imageVector = Icons.Default.Add, contentDescription = "Add")
            }
        }
    ) {innerPadding ->
        Column(modifier = Modifier.padding(innerPadding)
            .fillMaxHeight()
            .fillMaxWidth(),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally) {
            Text(text = "Click Count is $count",
                modifier = Modifier.padding(8.dp)
                    .fillMaxWidth(),
                textAlign = TextAlign.Center)
        }
    }
}
```

![Scaffold](/photos/scaffold.png)
