## LocalTimeクラスを使う

・LocalTime.of()メソッド

```kotlin
LocalTime.of(Int:~時,Int:~分)
```

例：スピナーから時間を取得

```kotlin
 startTime = LocalTime.of(
    binding.startHourSp.selectedItem.toString().toInt(),
    binding.startMinSp.selectedItem.toString().toInt()
    )
```

・差の計算

例

```kotlin
workTime = endTime.minusHours(startHour.toLong()).minusMinutes(startMinutes.toLong())
```

・minusHours(Long),minusMunutes(Long)のメソッドにそれぞれ引きたい時刻の数値を入力することで求められる

## TimeStamp

### TimeStampからyyyyy/MM/ddの形への変換

```kotlin
val date = Date(timestamp * 1000)
val format = SimpleDateFormat("yyyy/MM/dd")
val dateText = format.format(date)
```

・TimeStampの値はLong型で扱う

・Date型はms単位なので、TimeStampの値に1000をかけて扱う


