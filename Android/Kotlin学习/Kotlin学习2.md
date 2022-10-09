# Kotlin学习2

[TOC]

## 集合的创建与遍历

Kotlin提供了一个内置的listOf()函数来简化初始化集合的写法

```kotlin
val list = listOf("Apple", "Pear")
for (fruit in list) {
    println(fruit)
}
```

- **listOf()函数创建的是一个不可变的集合**

- **mutableListOf()函数可以创建可变集合**，用法和listOf类似

- **Set集合的用法和list一样，函数换成了setOf()和mutableSetOf()**,set集合是不可以存放重复元素的，如果存放了多个相同元素，只会保留其中一份

- Map集合是一种键值对形式的数据结构

  ```kotlin
  val map = HashMap<String, Int>()
  map["Apple"] = 1
  map["Banana"] = 2
  ```

  也可以利用mapOf简便

  ```
  val map = mapOf("Apple" to 1, "Pear" to 2)
  ```

  遍历也有所不同

  ```kotlin
  for ((fruit, number) in map) {
  	println("fruit is" + fruit , "number is " + number)
  }
  ```

## 集合的函数式API

### Lambda的化简

- **Lambda就是一小段可以作为参数传递的代码**

- 语法结构

  ```kotlin
  {参数名1: 参数类型, 参数名2: 参数类型 -> 函数体}
  ```

- 进行Lambda的简化推导

  1. ```kotlin
     val list = listOf("Apple", "Pear")
     val lambda = {fruit: String -> fruit.length }
     val maxLengthFruit = list.maxBy(lambda)
     ```

  2. ```kotlin
     val maxLengthFruit = list.maxBy({fruit: String -> fruit.length })
     ```

  3. Kotlin规定，当Lambda参数是函数的最后一个参数时，可以将Lambda表达式移到函数括号的外卖

     ```kotlin
     val maxLengthFruit = list.maxBy(){fruit: String -> fruit.length }
     ```

  4. 如果Lambda参数是函数唯一一个参数的话，可以省略函数的括号

     ```kotlin
     val maxLengthFruit = list.maxBy{fruit: String -> fruit.length }
     ```

  5. 由于Kotlin的类型推导机制，可以不用声明参数类型

     ```kotlin
     val maxLengthFruit = list.maxBy{fruit -> fruit.length }
     ```

  6. 只有一个参数的时候可以用it关键字代替

     ```kotlin
     val maxLengthFruit = list.maxBy{it.length}
     ```

### 常用函数API

- map函数用于将集合中的每个元素都映射成一个另外的值，规则在Lambda表达式中指定，最终生成一个新的集合,如让字符串全部大写如下

  ```kotlin
  val newList = list.map{it.toUpperCase()}
  ```

- filter函数是用来过滤集合中的数据的，可以单独使用，也可以配合map函数

  ```kotlin
  val newList = list.filter{it.length < 5}.map{it.toUpperCase}
  ```

- any函数用于判断集合中是否至少存在一个元素满足指定条件,all函数用于判断集合中是否所有元素都满足指定条件

  ```kotlin
  val anyResult = list.any{it.length < 5}
  val allResult = list.all{it.length < 5}
  ```

- java函数式API的使用，调用的java方法接收一个单抽象方法接口参数，就可以使用函数式API，java单抽象方法指的是接口中只有一个待实现的方法

- Kotlin实现匿名类的写法使用object关键字

  1. ```kotlin
     Thread(object: Runnable{
     	override fun run() {
     	println("Thread is running")
     	}
     }).start()
     ```

  2. 只有一个run方法需要实现，能够自动识别

     ```kotlin
     Thread(Runnable {
     	println("Thread is running")
     }).start()
     ```

  3. 方法中只有一个参数，可以将接口名省略

     ```kotlin
     Thread({
     	println("Thread is running")
     }).start()
     ```

  4. 括号可省，原因同上

     ```kotlin
     Thread{
     	println("Thread is running")
     }.start()
     ```

## 空指针检查

**Kotlin默认所有的参数和变量都不可为空，即Kotlin将空指针异常的检查提前到了编译时期**

如果需要某个参数或者变量为空，Kotlin提供了一套可为空的类型系统，不过需要在编译期就将所有潜在的空指针处理掉

**在类名的后面加上一个?即表示可以为空**

### 判空辅助工具

- **?.**操作符，当对象不为空时正常调用相应的方法，当对象为空时则什么都不做

如

```kotlin
if (a != null) {
	a.doSomething()
}
```

可以简化为

```kotlin
a?.doSomething()
```

- **?:**操作符左右两边都接收一个表达式，如果左边表达式的结果不为空就返回左边表达式的结果，否则就返回右边表达式的结果

```kotlin
val c = if (a != null) {
	a
}else {
	b
}
```

可以简化为

```kotlin
val c = a ?: b
```

- 在对象后面加上**!!**，表示确定非空，可以强行通过编译，但最好能不用就不用，后果自负

- 辅助工具**let**,是一个函数，提供了函数式API的编程接口，并将原始调用对象作为参数传递到Lambda表达式中

  ```kotlin
  obj.let { obj2 -> 编写具体的业务逻辑
  }
  ```

  obj2和obj是同一个对象，为了防止变量重名

  **let函数可以处理全局变量的判空问题**

## Kotlin技巧

### 字符串内嵌表达式

语法规则

```kotlin
"hello ${obj.name}.nice to meet you!"
```

表达式中只有一个变量的时候，可以将两边的大括号省略

### 函数的参数默认值

Kotlin支持函数设置默认参数值，没用传参给参数时使用设置的默认参数值，不必按照顺序来，如

```kotlin
fun printW(num: Int = 100, str: String) {
	println("num is $num, str is $str")
}
fun main() {
	printW(str = "world")
}
```

