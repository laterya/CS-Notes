# Activity的生命周期

## 返回栈

- 每启动一个新的Activity，就会在覆盖在原Activity上，然后点击Back键会销毁最上面的Activity，下面的一个Activity就会重新显示出来

- Android是使用任务（task）来管理Activity的，一个任务就是一组存放在栈里的Activity的集合，这个栈也被称作返回栈（back stack）

## Activity状态

**每个Activity在其生命周期中最多可能会有4种状态**

1. 运行状态：Activity位于返回栈的栈顶时即处于运行状态
2. 暂停状态：当Activity不再处于栈顶位置，但仍可见时，处于暂停状态
3. 停止状态：当Activity不再处于栈顶位置，而且完全不可见时，就进入了停止状态
4. 销毁状态：一个Activity从返回栈中移除后就变成了销毁状态

## Activity的生存期

![生命周期](https://s2.loli.net/2022/05/11/YCOEpw6inyRBG1e.jpg)

## Activity被回收解决方案

当Activity进入停止状态时，是有可能被系统回收的，按back键返回时数据信息并没有保留。

所以使用Activity中的onSavenInstanceState()回调方法，这个方法保证Activity在被回收之前一定会被调用

onSaveInstanceState()方法会携带一个Bundle类型的参数，Bundle提供了一系列的方法用于保存数据，比如可以使用putString()方法保存字符串，使用putInt()方法保存整型数据，以此类推。每个保存方法需要传入两个参数，第一个参数是键，用于后面从Bundle中取值，第二个参数是真正要保存的内容。

1. 在Activity保存数据

   ```kotlin
   override fun onSaveInstanceState(outState: Bundle) {
       super.onSaveInstanceState(outState)
       val tempData = "Something you just typed"
       outState.putString("data_key", tempData)
   }
   ```

2. 恢复数据

   onCreate()方法有Bundle类型的参数，这个参数**一般情况下是null**，但是如果在Activity被系统回收之前，**通过onSaveInstanceState()方法保存数据**，这个参数就会带有之前保存的全部数据，再通过相应的取值方法将数据取出即可。

   ```kotlin
   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       Log.d(tag, "onCreate")
       setContentView(R.layout.activity_main)
       if (savedInstanceState != null) {
           val tempData = savedInstanceState.getString("data_key")
           Log.d(tag, "tempData is $tempData")
       }
       ...
   }
   ```

Intent还可以结合Bundle一起用于传递数据。首先我们可以把需要传递的数据都保存在Bundle对象中，然后再将Bundle对象存放在Intent里。到了目标Activity之后，先从Intent中取出Bundle，再从Bundle中一一取出数据。