# XMLデータ取得例

## 1

・元データ

```xml
<city_list>
    <city type="current">
        <name>New Taipei City</name>
        <name_tw>新北市</name_tw>
        <file_name>current.xml</file_name>
    </city>
    <city>
        <name>Taipei City</name>
        <name_tw>台北市</name_tw>
        <file_name>taipei.xml</file_name>
    </city>
    <city>
        <name>Taoyuan City</name>
        <name_tw>桃園市</name_tw>
        <file_name>taoyuan.xml</file_name>
    </city>
    <city>
        <name>Taichung City</name>
        <name_tw>台中市</name_tw>
        <file_name>taichung.xml</file_name>
    </city>
    <city>
        <name>Tainan City</name>
        <name_tw>台南市</name_tw>
        <file_name>tainan.xml</file_name>
    </city>
</city_list>
```

・取得コード

```kotlin
 private fun getCityList(input: InputStream): MutableList<City>{
        var cityList = mutableListOf<City>()
        var currentData = City()
        var currentName = ""
        val factory = XmlPullParserFactory.newInstance()
        val parser = factory.newPullParser()
        parser.setInput(input,"UTF-8")
        var eventType = parser.eventType
        while (eventType != XmlPullParser.END_DOCUMENT){
            if (cityList.isNotEmpty()) currentData = cityList.last()
            if (eventType == XmlPullParser.START_TAG){
                when(parser.name){
                    "name" -> {
                        cityList.add(City())
                        currentName = "name"
                    }
                    "name_tw" -> currentName = "name_tw"
                    "file_name" -> currentName = "file_name"
                }
            }
            else if (eventType == XmlPullParser.TEXT){
                when(currentName){
                    "name" -> if (parser.text.isNotBlank()) currentData.name = parser.text
                    "name_tw" -> if (parser.text.isNotBlank()) currentData.name_tw = parser.text
                    "file_name" -> if (parser.text.isNotBlank()) currentData.file_name = parser.text
                }
            }
            eventType = parser.next()
        }
        input.close()
        return cityList
    }

// Model class
data class City(var name: String = "", var name_tw: String = "", var file_name: String = "")
```

## 2

・元データ

```xml
<weather_data>
    <current_weather>
        <city>Taichung City</city>
        <latitude>24.1477°</latitude>
        <longitude>120.6736°</longitude>
    </current_weather>
    <hourly_forecast>
        <hour>
            <time>00:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>22°</temperature>
        </hour>
        <hour>
            <time>01:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>21°</temperature>
        </hour>
        <hour>
            <time>02:00</time>
            <weather_condition>overcast</weather_condition>
            <temperature>20°</temperature>
        </hour>
        <hour>
            <time>03:00</time>
            <weather_condition>rain</weather_condition>
            <temperature>19°</temperature>
        </hour>
        <hour>
            <time>04:00</time>
            <weather_condition>rain</weather_condition>
            <temperature>18°</temperature>
        </hour>
        <hour>
            <time>05:00</time>
            <weather_condition>thunder</weather_condition>
            <temperature>17°</temperature>
        </hour>
        <hour>
            <time>06:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>19°</temperature>
        </hour>
        <hour>
            <time>07:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>21°</temperature>
        </hour>
        <hour>
            <time>08:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>23°</temperature>
        </hour>
        <hour>
            <time>09:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>24°</temperature>
        </hour>
        <hour>
            <time>10:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>25°</temperature>
        </hour>
        <hour>
            <time>11:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>26°</temperature>
        </hour>
        <hour>
            <time>12:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>27°</temperature>
        </hour>
        <hour>
            <time>13:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>28°</temperature>
        </hour>
        <hour>
            <time>14:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>27°</temperature>
        </hour>
        <hour>
            <time>15:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>26°</temperature>
        </hour>
        <hour>
            <time>16:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>25°</temperature>
        </hour>
        <hour>
            <time>17:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>24°</temperature>
        </hour>
        <hour>
            <time>18:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>23°</temperature>
        </hour>
        <hour>
            <time>19:00</time>
            <weather_condition>cloudy</weather_condition>
            <temperature>22°</temperature>
        </hour>
        <hour>
            <time>20:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>21°</temperature>
        </hour>
        <hour>
            <time>21:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>20°</temperature>
        </hour>
        <hour>
            <time>22:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>19°</temperature>
        </hour>
        <hour>
            <time>23:00</time>
            <weather_condition>sunny</weather_condition>
            <temperature>18°</temperature>
        </hour>
    </hourly_forecast>
    <ten_day_forecast>
        <day>
            <date>2024-03-20</date>
            <weather_condition>sunny</weather_condition>
            <high_temperature>30°C</high_temperature>
            <low_temperature>22°C</low_temperature>
        </day>
        <day>
            <date>2024-03-21</date>
            <weather_condition>cloudy</weather_condition>
            <high_temperature>29°C</high_temperature>
            <low_temperature>21°C</low_temperature>
        </day>
        <day>
            <date>2024-03-22</date>
            <weather_condition>thunder</weather_condition>
            <high_temperature>27°C</high_temperature>
            <low_temperature>20°C</low_temperature>
        </day>
        <day>
            <date>2024-03-23</date>
            <weather_condition>cloudy</weather_condition>
            <high_temperature>28°C</high_temperature>
            <low_temperature>21°C</low_temperature>
        </day>
        <day>
            <date>2024-03-24</date>
            <weather_condition>sunny</weather_condition>
            <high_temperature>30°C</high_temperature>
            <low_temperature>22°C</low_temperature>
        </day>
        <day>
            <date>2024-03-25</date>
            <weather_condition>sunny</weather_condition>
            <high_temperature>31°C</high_temperature>
            <low_temperature>23°C</low_temperature>
        </day>
        <day>
            <date>2024-03-26</date>
            <weather_condition>cloudy</weather_condition>
            <high_temperature>29°C</high_temperature>
            <low_temperature>21°C</low_temperature>
        </day>
        <day>
            <date>2024-03-27</date>
            <weather_condition>rain</weather_condition>
            <high_temperature>25°C</high_temperature>
            <low_temperature>19°C</low_temperature>
        </day>
        <day>
            <date>2024-03-28</date>
            <weather_condition>sunny</weather_condition>
            <high_temperature>30°C</high_temperature>
            <low_temperature>22°C</low_temperature>
        </day>
        <day>
            <date>2024-03-29</date>
            <weather_condition>cloudy</weather_condition>
            <high_temperature>28°C</high_temperature>
            <low_temperature>21°C</low_temperature>
        </day>
        <day>
            <date>2024-03-30</date>
            <weather_condition>rain</weather_condition>
            <high_temperature>27°C</high_temperature>
            <low_temperature>20°C</low_temperature>
        </day>
    </ten_day_forecast>
    <air_quality_index>
        <current_aqi>86</current_aqi>
    </air_quality_index>
</weather_data>
```

・取得コード

```kotlin
 private fun getWeatherData(input: InputStream): Weather {
        val factory = XmlPullParserFactory.newInstance()
        val parser = factory.newPullParser()
        parser.setInput(input, "UTF-8")
        var eventType = parser.eventType
        var currentTag = ""
        var isHourData = true
        var data = Weather()
        val dayList = mutableListOf<DayWeather>()
        val hourList = mutableListOf<TimeWeather>()
        var currentDayData = DayWeather()
        var currentHourData = TimeWeather()
        while (eventType != XmlPullParser.END_DOCUMENT) {
            if (dayList.isNotEmpty()) currentDayData = dayList.last()
            if (hourList.isNotEmpty()) currentHourData = hourList.last()

            if (eventType == XmlPullParser.START_TAG) {
                when (parser.name) {
                    "city" -> currentTag = "city"
                    "latitude" -> currentTag = "latitude"
                    "longitude" -> currentTag = "longitude"
                    "current_aqi" -> currentTag = "current_aqi"
                    "time" -> {
                        currentTag = "time"
                        hourList.add(TimeWeather())
                    }

                    "weather_condition" -> currentTag = "weather_condition"
                    "temperature" -> currentTag = "temperature"
                    "ten_day_forecast" -> isHourData = false
                    "date" -> {
                        currentTag = "date"
                        dayList.add(DayWeather())
                    }

                    "high_temperature" -> currentTag = "high_temperature"
                    "low_temperature" -> currentTag = "low_temperature"
                }
            } else if (eventType == XmlPullParser.TEXT && parser.text.isNotBlank()) {
                when (currentTag) {
                    "city" -> data.city = parser.text
                    "latitude" -> data.latitude = parser.text
                    "longitude" -> data.longitude = parser.text
                    "current_aqi" -> data.air_quality = parser.text
                    "time" -> currentHourData.time = parser.text
                    "weather_condition" -> {
                        if (isHourData) {
                            currentHourData.weather_condition = parser.text
                        } else currentDayData.weather_condition = parser.text
                    }

                    "temperature" -> currentHourData.temperature = parser.text
                    "date" -> currentDayData.date = parser.text
                    "high_temperature" -> currentDayData.high_temperature = parser.text
                    "low_temperature" -> currentDayData.low_temperature = parser.text
                }
            }
            eventType = parser.next()
        }
        data.todayWeather = hourList
        data.dayWeather = dayList
        input.close()
        return data
    }

//Model class
data class TimeWeather(
    var time: String = "",
    var weather_condition: String = "",
    var temperature: String = ""
)

data class DayWeather(
    var date: String = "",
    var weather_condition: String = "",
    var high_temperature: String = "",
    var low_temperature: String = ""
)

data class Weather(
    var city: String = "",
    var latitude: String = "",
    var longitude: String = "",
    var todayWeather: MutableList<TimeWeather> = mutableListOf(),
    var dayWeather: MutableList<DayWeather> = mutableListOf(),
    var air_quality: String = ""
)
```
