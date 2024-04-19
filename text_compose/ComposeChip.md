# Chip

・ガイド、フィルタ、入力、候補など様々な要素の補助として使用

・4つの種類が存在する

### パラメータ

・`label`:表示する文字列

・`icon`:表示するアイコン、`leadingIcon`,`trailIcon`の二つのパラメータがあるものもある

・`onClick`:押した時の動作

## 実装例

```kotlin
fun ChipSample(){
    var isSelected by remember {
        mutableStateOf(false)
    }
    var isEnabled by remember {
        mutableStateOf(true)
    }
    Column() {
        //Assist
        AssistChip(onClick = {  }, label = {
            Text(text = "Assist Chip")
        },
            leadingIcon ={
                Icon(Icons.Filled.Settings,
                    contentDescription = null,
                    modifier = Modifier.size(AssistChipDefaults.IconSize))
            },
            modifier = Modifier.padding(8.dp))
        //Filter
        FilterChip(selected = isSelected,
            onClick = { isSelected = !isSelected },
            label = {
                Text(text = "Filter Chip")
            },
            leadingIcon = {
                if (isSelected) {
                Icon(
                    imageVector = Icons.Filled.Done,
                    contentDescription = null,
                    modifier = Modifier.size(FilterChipDefaults.IconSize)
                )
            }
                else {
                    null
                }
            },
            modifier = Modifier.padding(10.dp)
        )
        //Input
        if (isEnabled){
            InputChip(selected = isEnabled,
                onClick = { isEnabled = !isEnabled },
                label = {
                    Text(text = "enter user text")
                },
                avatar = {
                    Icon(imageVector = Icons.Filled.AccountCircle,
                        contentDescription = null,
                        modifier = Modifier.size(InputChipDefaults.AvatarSize))
                },
                trailingIcon = {
                    Icon(imageVector = Icons.Filled.Close,
                        contentDescription = null,
                        modifier = Modifier.size(InputChipDefaults.IconSize))
                },
                modifier = Modifier.padding(10.dp)
                )
        }
        //Suggestion
        SuggestionChip(onClick = { /*TODO*/ },
            label = {
                Text(text = "Suggestion Chip")
            },
            modifier = Modifier.padding(10.dp))
    }
}
```

![chip](/photos/chip.png)
