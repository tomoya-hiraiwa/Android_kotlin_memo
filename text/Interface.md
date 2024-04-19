# InterFace

## 概要

・抽象メソッドを提供することができる

→`abstract`と同じ能力

### abstractとの違い

・コンストラクタを持てない(クラスじゃないから)

抽象クラスの場合

```kotlin
abstract class CHoge(val text: String){
    abstract fun useHoge()
    abstract fun changeHoge(): String
}
```

このように、クラスなのでコンストラクタを持てる

Interfaceの場合

```kotlin
interface Hoge{
    fun useHoge (text: String)
    fun changeHoge(text: String): String
}
```

クラスじゃないのでコンストラクタを持てない

・メソッドに`abstract`修飾子をつける必要がない

→定義したメソッドはすべて抽象メソッドになるため

・複数のInterfaceを実装（継承）できる

```kotlin
class UseHoge:Hoge, Hoge2{
    override fun useHoge(text: String) {
        println(text)
    }

    override fun changeHoge(text: String): String {
        val newText = text.replace("t","T")
        return newText
    }

    override fun printHoge() {
        println("print Hoge")
    }

}

interface Hoge{
    fun useHoge (text: String)
    fun changeHoge(text: String): String
}

interface Hoge2{
    fun printHoge()
}
```

このように、UseHogeクラスは2つのInterfaceを実装している

## 具体例

```kotlin
fun main(args: Array<String>) {
    UseHoge().useHoge("use interface")
    val text = UseHoge().changeHoge("text is this")
    println(text)
    UseHoge().printHoge()
}
class UseHoge:Hoge, Hoge2{
    override fun useHoge(text: String) {
        println(text)
    }

    override fun changeHoge(text: String): String {
        val newText = text.replace("t","T")
        return newText
    }

    override fun printHoge() {
        println("print Hoge")
    }

}

interface Hoge{
    fun useHoge (text: String)
    fun changeHoge(text: String): String
}

interface Hoge2{
    fun printHoge()
}
```

・出力

```
use interface
TexT is This
print Hoge
```


