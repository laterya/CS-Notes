# Kotlin学习

[TOC]

## 基础语法

### 变量

只有两种关键字：val和var

- val用来声明一个不可变的变量
- var用来声明一个可变的变量

### 函数

fun 是定义函数的关键字，不论声明定义什么函数，都要使用fun来声明

```kotlin
fun methodName(param1: Int, param2: Int): Int {		//括号后部分表示返回类型，可选
    //参数的声明格式是"参数名：参数类型"
	return 0
}
```

当一个函数中只有一行代码时，可以不写函数体，将唯一一行代码写在函数定义尾部，用等号连接

例如：

```kotlin
fun largerNumber(num1: Int, num2: Int): Int = max(num1, num2)
```

### 逻辑控制

### if条件语句

if语句可以有返回值，返回值为if语句每一个条件中最后一行代码的返回值

```kotlin
fun largerNumber(num1: Int, nums: Int) = if (num1 > num2) num1 else num2
```

### when条件语句

when语句和if语句一样，也是可以有返回值的，类似于java，c++的switch

**when语句允许传入一个任意类型的参数，然后再when的结构体中定义一系列的条件**

用例：

```kotlin
fun getScore(name: String) = when (name) {
	"Tom" ->{86}
	"Jack" -> {77}
	"Jim" -> 99
	else -> 0
	//执行逻辑只有一行代码时，{}可省略
}
```

**when语句可以不带参数**，用以实现一些特殊场景

```kotlin
fun getScore(name: String) = when {
	name.startWith("Tom") -> 86
	name == "Jack" ->99
	else -> 0
	//由此实现了所有以Tom开头的人对应分数都为86
}
```

### 循环语句

while语句和其他语言c++，java一样的使用方法

for的用法被加强，变成了for-in循环

```kotlin
fun main() {
	for (i in 0..10) {	// 在..两边即为指定闭区间
	printlen(i)
	//遍历区间每一个元素
	}
}
```

- 用**until关键字代替..关键字，左闭右闭区间变为左闭右开区间**

- step关键字可以跳过循环中的元素

- downTo关键字可以创建降序的闭区间

## 面向对象编程

### 类与对象

- 用class关键字来声明一个类

- 实例化一个类的方式和java类似，去掉了new关键字

  ```kotlin
  val p = Person()继承与构造函数
  ```

### 继承与构造函数

#### 继承

1. 使父类可继承，在Kotlin中任何一个非抽象类都是不可以被继承的，除非加上open关键字

   ```kotlin
   open class Person {
   	...
   }
   ```

2. 让子类继承父类，用符号**:**

   ```kotlin
   class Student : Person() {
   	...
   }
   ```

#### 构造函数

- Kotlin将构造函数分为两种，主构造函数和次构造函数

- 每个类默认都有一个不带参数的主构造函数，也可以显式地给它指明参数

- 主构造函数没有函数体，直接定义在类名后面,init结构体可提供主构造函数的逻辑

  ```kotlin
  class Student(val son:String, val garde: Int) : Person() {
  	init {
  	...
  	}
  }
  ```

- 子类的构造函数必须调用父类中的构造函数，Kotlin中采用括号实现

**次构造函数**

- 次构造函数通过**constructor**关键字来定义

- 当一个类中既有主构造函数又有次构造函数时，所有的次构造函数都必须调用主构造函数(包括间接调用)

  ```kotlin
  class Student(val son: String, val grade: Int, name: String, age: Int) : Person(name, age) {
  	constructor(name: String, age: Int) :this("", 0, name, age)	//调用主构造函数
  	constructor(): this("", 0)	//通过调用上一个次构造函数间接调用主构造函数
  }
  ```

### 接口

- Kotlin是单继承结构，最多只能继承一个父类

- 可以实现任意多个接口

- 要实现的接口在类定义时使用冒号，中间使用逗号分隔

  ```kotlin
  class Student(name: Sting, age: Int) : Person(), Study {}
  ```

- Kotlin运行对接口中定义的函数进行默认实现

### 数据类与单例类

- 在一个类前面声明data关键字时，表明希望这个类是一个数据类，Kotlin会自动生成equals(),hashCode()等固定的方法

  ```kotlin
  data class Cellphone(val brand: String, val price: Double)
  ```

**单例化**

- 将class关键字改成objec即可创建一个单例类

  ```kotlin
  object Singleton {}
  ```

-----------------------

