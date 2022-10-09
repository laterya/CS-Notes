## Fragment的简单用法

1.   新建布局文件如left_fragment.xml

     ```xml
     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <Button
      android:id="@+id/button"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_gravity="center_horizontal"
      android:text="Button"
      />
     </LinearLayout>
     ```

2.   新建Fragment类，并继承Fragment

     ```kotlin
     class LeftFragment : Fragment() {
      override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
      savedInstanceState: Bundle?): View? {
      return inflater.inflate(R.layout.left_fragment, container, false)
      }
     }
     ```

     这里仅仅是重写了Fragment的onCreateView()方法，然后在这个方法中通过LayoutInflflater的inflate()方法将刚才定义的left_fragment布局动态加载进来

3.   接下来修改activity_main.xml中的代码，将fragment放入布局即可


### 动态添加Fragment

1.   将要动态加入Fragment的地方加入FrameLayout布局，所有控件默认都会摆在布局的左上角

2.   在代码中向FrameLayout里添加内容，从而实现动态添加Fragment的功能

     ```kotlin
     class MainActivity : AppCompatActivity() {
      override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)
      setContentView(R.layout.activity_main)
      button.setOnClickListener {
      replaceFragment(AnotherRightFragment())
      }
      replaceFragment(RightFragment())
      }
      private fun replaceFragment(fragment: Fragment) {
      val fragmentManager = supportFragmentManager
      val transaction = fragmentManager.beginTransaction()
      transaction.replace(R.id.rightLayout, fragment)
      transaction.commit()
      }
     }
     ```

     1.   创建待添加Fragment的实例

     2.    获取FragmentManager，在Activity中可以直接调用getSupportFragmentManager()方法获取。

     3.    开启一个事务，通过调用beginTransaction()方法开启。

     4.    向容器内添加或替换Fragment，一般使用replace()方法实现，需要传入容器的id和待添加的Fragment实例。

     5.    提交事务，调用commit()方法来完成。

### 在**Fragment**中实现返回栈

FragmentTransaction中提供了一个addToBackStack()方法，可以用于将一个事务添加到返回栈中。

```kotlin
class MainActivity : AppCompatActivity() {
 ...
 private fun replaceFragment(fragment: Fragment) {
 val fragmentManager = supportFragmentManager
 val transaction = fragmentManager.beginTransaction()
 transaction.replace(R.id.rightLayout, fragment)
 transaction.addToBackStack(null)
 transaction.commit()
 }
}
```

### **Fragment**和**Activity**之间的交互

-   为了方便Fragment和Activity之间进行交互，FragmentManager提供了一个类似于findViewById()的方法，专门用于从布局文件中获取Fragment的实例

    ```kotlin
    val fragment = supportFragmentManager.findFragmentById(R.id.leftFrag) as LeftFragment
    ```

    

-   在每个Fragment中都可以通过调用getActivity()方法来得到和当前Fragment相关联的Activity实例

    ```kotlin
    if (activity != null) {
     val mainActivity = activity as MainActivity
    }
    ```

    当Fragment中需要使用Context对象时，也可以使用getActivity()方法，因为获取到的Activity本身就是一个Context对象。

-   不同的Fragment之间可以通信，

-   它的基本思路非常简单：首先在一个Fragment中

    可以得到与它相关联的Activity，然后再通过这个Activity去获取另外一个Fragment的实例，这样就实现了不同Fragment之间的通信功能。

## **Fragment**的生命周期 

-   运行状态

    当一个Fragment所关联的Activity正处于运行状态时，该Fragment也处于运行状态。

-   暂停状态

    当一个Activity进入暂停状态时（由于另一个未占满屏幕的Activity被添加到了栈顶），与它相关联的Fragment就会进入暂停状态

-    停止状态

    当一个Activity进入停止状态时，与它相关联的Fragment就会进入停止状态，或者通过调用FragmentTransaction的remove()、replace()方法将Fragment从Activity中移除，但在事务提交之前调用了addToBackStack()方法，这时的Fragment也会进入停止状态。总的来说，进入停止状态的Fragment对用户来说是完全不可见的，有可能会被系统回收

-   销毁状态

    Fragment总是依附于Activity而存在，因此当Activity被销毁时，与它相关联的Fragment就会进入销毁状态。或者通过调用FragmentTransaction的remove()、

    replace()方法将Fragment从Activity中移除，但在事务提交之前并没有调用addToBackStack()方法，这时的Fragment也会进入销毁状态。

回调方法：

onAttach()：当Fragment和Activity建立关联时调用。

onCreateView()：为Fragment创建视图（加载布局）时调用。

onActivityCreated()：确保与Fragment相关联的Activity已经创建完毕时调用。

onDestroyView()：当与Fragment关联的视图被移除时调用。

onDetach()：当Fragment和Activity解除关联时调用。

![image-20220518205356859](https://s2.loli.net/2022/05/18/4ujcbPrAhvE9NIg.png)

## 动态加载布局的技巧

### 使用限定符

large就是一个限定符，那些屏幕被认为是large的设备就会自动加载layout-large文件夹下的布局，小屏幕的设备则还是会加载layout文件夹下的布局

![image-20220518205847474](https://s2.loli.net/2022/05/18/huPzvjnatJGRpVN.png)![image-20220518205901031](https://s2.loli.net/2022/05/18/FCkwmasSVo4H23z.png)

### 使用最小宽度限定符

最小宽度限定符允许我们对屏幕的宽度指定一个最小值（以dp为单位），然后以这个最小值为临界点，屏幕宽度大于这个值的设备就加载一个布局，屏幕宽度小于这个值的设备就加载另一个布局

1.   在res目录下新建layout-sw600dp文件夹，然后在这个文件夹下新建activity_main.xml布局
2.   这就意味着，当程序运行在屏幕宽度大于等于600 dp的设备上时，会加载layou-sw600dp/activity_main布局，当程序运行在屏幕宽度小于600 dp的设备上时，则仍然加载默认的layout/activity_main布局。