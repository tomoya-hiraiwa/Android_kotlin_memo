# SerchView

## レイアウト例

![searchBar](/photos/SearchBar.png)

```kotlin
@Composable
fun SearchBar(
    modifier: Modifier = Modifier
) {
    TextField(value = "",
        onValueChange = {},
        //検索アイコンを指定
        leadingIcon = {
                      Icon(imageVector = Icons.Default.Search, contentDescription = null)
        },
        colors = TextFieldDefaults.colors(
            unfocusedContainerColor = MaterialTheme.colorScheme.surface,
            focusedContainerColor = MaterialTheme.colorScheme.surface
        ),
        //空欄時に表示されるテキストを指定
        placeholder = {
                      Text(stringResource(id = R.string.placeholder_search))
        },
        modifier = modifier
            .fillMaxWidth()
            .heightIn(min = 56.dp))
}
```
