# abstract(抽象)について

## 概要

 ・`abstract class`(抽象クラス)

 ・`abstract fun`(抽象メソッド)

 がある

 ```kotlin
abstract class Hoge(){}
```

と記述することで、抽象クラスにすることができる。

```kotlin
abstract fun hoge(){}
```

と記述することで、抽象メソッドにできる

### 特徴

・直接インスタンス化して使用することはせず、他のクラスに継承して使用

・抽象メソッドは、抽象クラス内のみ（interface除く）で定義できる

・抽象クラスを継承した時は、クラス内の抽象メソッドは必ず実装しなければならない

### 具体例

・抽象クラスであるHogeクラスを作成する

```kotlin
abstract class Hoge{
    abstract fun callHoge(text: String)
fun useHoge(){
    println("call use Hoge function")
  }
}
```

このクラスには、

  ・callHogeメソッド（抽象メソッド）

  ・useHogeメソッド

の二つのメソッドが存在する。

・このクラスは抽象クラスなので直接インスタンス化することはできない

```kotlin
fun main(args: Array<String>){
  Hoge() // Cannot create instance of an abstract class error
}
```

・このクラスを継承したクラスを作成する

Hoge1

```kotlin
class Hoge1(): Hoge() {
    override fun callHoge(text: String) {
        println(text)
    }
}
```

このようにcallHogeメソッドをかならずoverrideする必要がある

Hoge2

```kotlin
class Hoge2(): Hoge(){
    override fun callHoge(text: String) {
        repeat(2){
            println(text)
        }
    }
}
```

具体的な動作内容は、継承先のクラス内で記述する

・これらのクラスを呼び出す

```kotlin
fun main(args: Array<String>) {
    Hoge1().callHoge("using Hoge function")
    Hoge2().callHoge("using Hoge2 function")
    Hoge2().useHoge()
}
```

出力

```
using Hoge function
using Hoge2 function
using Hoge2 function
call use Hoge function
```
