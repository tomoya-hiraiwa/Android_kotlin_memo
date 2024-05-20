# CSVファイルからデータを取得する

### 基本コード

```kotlin
 private fun getData(input: InputStream): MutableList<Any>{
        val dataList = mutableListOf<Any>()
        val inputReader = InputStreamReader(input)
        val bufferReader = BufferedReader(inputReader)
        //1行ごと読み込んだStringのリストを取得
        val lines = bufferReader.readLines()
        //行数分繰り返し
        for (i in lines.indices){
            //1行目は列名の場合は取得しないようにする
            if(i != 0){
                //列ごとに切り分ける
                val value = lines[i].split(",")
                //get data from value
            }
        }
        bufferReader.close()
        return dataList
    }
```

実装例

・データクラスの準備

```kotlin
data class CityData(
    val city: String = "",
    val city_ascii: String = "",
    val lat: String = "",
    val lng: String = "",
    val country: String = "",
    val Iso2: String = "",
    val Iso3: String = "",
    val admin_name: String = "",
    val capital: String = "",
    val population: String = "",
    val Id: String = ""
)
```

・取得メソッド

```kotlin
 private fun getDataFromCsv(input: InputStream): MutableList<CityData> {
        val dataList = mutableListOf<CityData>()
        val inputReader = InputStreamReader(input)
        val bufferReader = BufferedReader(inputReader)
        val lines = bufferReader.readLines()
        for (i in lines.indices) {
            if (i != 0) {
                val value = lines[i].split("\",")
                println(value)
                val data = CityData(
                    city = split(value[0]),
                    city_ascii = split(value[1]),
                    lat = split(value[2]),
                    lng = split(value[3]),
                    country = split(value[4]),
                    Iso2 = split(value[5]),
                    Iso3 = split(value[6]),
                    admin_name = split(value[7]),
                    capital = split(value[8]),
                    population = split(value[9]),
                    Id = split(value[10])
                )
                dataList.add(data)
            }
        }
        bufferReader.close()
        return dataList
    }
    private fun split(text: String): String{
        return text.replace("\"","")
    }
```

※今回のデータでは文字列内でも","が使用されていたため、取り方を変更している。
