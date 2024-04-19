#レイアウトについて

## Column

・要素を縦並びで配置

```kotlin
  Column {
        Text(text = "Alfred Sisley")
        Text(text = "3 minutes ago")
    }
```

### レイアウトの属性

・`verticalArrangement`と`horizonalAlignment`

```kotlin
 Column(verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally) {...}
```

## Row 

・要素を横並びで配置

```kotlin
Row {
        Text(text = "Alfred Sisley")
        Text(text = "3 minutes ago")
    }
```

### レイアウトの属性

・`verticalAlignment`と`horizonalArrangement`

```kotlin
Row(verticalAlignment = Alignment.CenterVertically,
       horizontalArrangement = Arrangement.Center) {...}
```

## Box

・要素を重ねて表示

```kotlin
   Box {
        Text(text = "Alfred Sisley")
        Text(text = "3 minutes ago")
    }
```

