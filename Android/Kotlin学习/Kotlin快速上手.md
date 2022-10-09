# Kotlin快速上手

## 基础语法

1. var: 可变, val :不可变的变量

   ```kotlin
   var name: String = "hello"	//String可省略，kolin会自行推导
   val age = 18
   ```

   

2. 类型之后加问好表示可为空

   ```kotlin
   var name: String? = null	//true
   var name: String = null		//false
   var name1: String = "Hello"
   //name1和name属于不同的类型
   name1 = name!!	//两个!表示强制类型转换
   name = name1	//name表示可为空，也可以不为空，故不用强制转换
   ```

3. 函数

   ```kotlin
   fun printfLen(str: String): String {
   //括号内为参数，括号外的string为返回类型
       println("$str")		//可以在字符串中使用$来引用变量
   	.....
   }
   ```

## 与java代码互调

### 语法变化

函数直接写在文件里而非类中

匿名内部类的创建

```kotlin
object Test {
	fun sayMessage(msg: String) {
	println(msg)
	}
}
Test.sayMessage("hello")
```

调用java的class,需要在class后加**.java**

