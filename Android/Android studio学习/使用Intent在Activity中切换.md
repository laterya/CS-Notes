# 使用Intent在Activity中切换

Intent是Android程序中各组件之间**进行交互**的一种重要方式，它不仅**可以指明当前组件想要执行的动作**，还可以在**不同组件之间传递数据**。Intent一般可**用于启动Activity、启动Service以及发送广播等场景**

## 使用显式Intent

Intent有多个构造函数的重载，其中一个是Intent(Context packageContext, Class<?> cls)。这个构造函数接收两个参数：第一个参数**Context要求提供一个启动Activity的上下文**；第二个参数**Class用于指定想要启动的目标Activity**，通过这个构造函数就可以构建出Intent的“意图”。Activity类中提供了一个startActivity()方法，专门用于启动Activity，它接收一个Intent参数，即可启动目标Activity。

```kotlin
button1.setOnClickListener {
    val intent = Intent(this, SecondActivity::class.java)
    startActivity(intent)
}
```

## 使用隐式Intent

隐式Intent并不明确指出想要启动哪个Activity，而是指定了一系列更为抽象的action和category等信息，然后由系统取分析这个Intent，并找出合适（可以响应）的Activity去启动

通过在<activity>标签下配置<intent-filter>的内容，可以指定当前Activity能够响应的action和category，打开AndroidManifest.xml，添加如下代码：

```kotlin
activity android:name=".SecondActivity" >
    <intent-filter>
        <action android:name="com.example.activitytest.ACTION_START" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

**只有<action>和<category>中的内容同时匹配Intent中指定的action和category时，这个Activity才能响应该Intent。**

使用隐式intent代码如下

```kotlin
button1.setOnClickListener {
    val intent = Intent("com.example.activitytest.ACTION_START")
    startActivity(intent)
}
```

**android.intent.category.DEFAULT是一种默认的category**，在调用startActivity()方法的时候会自动将这个category添加到Intent中。

**每个Intent只能指定一个action，但能指定多个category**。可以调用Intent中的addCategory()方法来添加一个category.

## 隐式Intent的更多用法

**使用Intent，不仅可以启动自己程序的Activity，还可以启动其他程序的Activity**。如下

```kotlin
button1.setOnClickListener {
    val intent = Intent(Intent.ACTION_VIEW)
    intent.data = Uri.parse("https://www.baidu.com")
    startActivity(intent)
}
```

首先指定了Intent的action是**Intent.ACTION_VIEW**，这是一个Android系统内置的动作，其常量值为android.intent.action.VIEW。然后通过**Uri.parse()方法将一个网址字符串解析成一个Uri对象**，再调用**Intent的setData()方法将这个Uri对象传递进去**。Kotlin的语法看上去像是给Intent的data属性赋值一样。

setDate()接收一个Uri对象，主要用于指定当前Intent正在操作的数据。

我们还可以在<intent-filter>标签中再配置一个<data>标签，用于更精确地指定当前Activity能够响应的数据。<data>标签中主要可以配置以下内容。
● android:scheme。用于指定数据的协议部分，如上例中的https部分。
● android:host。用于指定数据的主机名部分，如上例中的www.baidu.com部分。
● android:port。用于指定数据的端口部分，一般紧随在主机名之后。
● android:path。用于指定主机名和端口之后的部分，如一段网址中跟在域名之后的内容。
● android:mimeType。用于指定可以处理的数据类型，允许使用通配符的方式进行指定。
只有当<data>标签中指定的内容和Intent中携带的Data完全一致时，当前Activity才能够响应该Intent。不过，在<data>标签中一般不会指定过多的内容。

## 向下一个Activity传递数据

Intent中提供了一系列putExtra()方法的重载，可以把我们想要传递的数据暂存在Intent中，在启动另一个Activity后，只需要把这些数据从Intent中取出就可以了

```kotlin
button1.setOnClickListener {
    val data = "Hello SecondActivity"
    val intent = Intent(this, SecondActivity::class.java)
    intent.putExtra("extra_data", data)
    startActivity(intent)
}
```

使用显式Intent的方式来启动SecondActivity，并通过putExtra()方法传递了一个字符串。注意，这里putExtra()方法接收两个参数，**第一个参数是键，用于之后从Intent中取值，第二个参数才是真正要传递的数据**。
然后在SecondActivity中将传递的数据取出，并打印出来

```kotlin
class SecondActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.second_layout)
        val extraData = intent.getStringExtra("extra_data")
        Log.d("SecondActivity", "extra data is $extraData")
    }

}
```

## 返回数据给上一个Activity

Activity类中还有一个用于**启动Activity的startActivityForResult()方法**，但它期望在**Activity销毁的时候能够返回一个结果给上一个Activity**

1. **使用startActivityForResult()方法方法启动下一个activity**

startActivityForResult()方法接收两个参数：第一个参数还是Intent；第二个参数是请求码，用于在之后的回调中判断数据的来源。

```kotlin
button1.setClickListener {
	val intent = Intent(this, SecondActivity::class.java)
	startActivityForResult(intent, 1)
}
```

启动了SecondActivity,**请求码只要是一个唯一值即可**，接下来看Activity2的返回数据操作

2. **Activity2返回数据**

```kotlin
class SecondActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.second_layout)
        button2.setOnClickListener {
            val intent = Intent()
            intent.putExtra("data_return", "Hello FirstActivity")
            setResult(RESULT_OK, intent)
            finish()
        }
    }

}
```

setResult()方法专门用于向上一个activity返回数据。setResult()方法接收两个参数：**第一个参数用于向上一个Activity返回处理结果，一般只使用RESULT_OK或RESULT_CANCELED这两个值**；**第二个参数则把带有数据的Intent传递回去**。最后调用了finish()方法来销毁当前Activity。

3. **在上一个activity中接收传回来的数据**

由于是使用startActivityForResult()方法来启动SecondActivity的，在SecondActivity被销毁之后会回调上一个Activity的onActivityResult()方法，因此我们需要在FirstActivity中重写这个方法来得到返回的数据

```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)
    when (requestCode) {
        1 -> if (resultCode == RESULT_OK) {
            val returnedData = data?.getStringExtra("data_return")
            Log.d("FirstActivity", "returned data is $returnedData")
        }
    }
}
```

onActivityResult()方法带有3个参数：**第一个参数requestCode，即我们在启动Activity时传入的请求码**；第二个参数resultCode，即我**们在返回数据时传入的处理结果**；第三个参数data，即携带着返回数据的Intent。

4. **如果不是通过按钮返回FirstActivity的话，需要在SecondActivity中重写onBackPressed()方法来达到相同的效果。**

```kotlin
override fun onBackPressed() {
	val intent = Intent()
	intent.putExtra("return_data", "返回的字符串")
	setResult(RESULT_OK, intent)
	finish()
}
```

