# Activity学习

## Activity的基本用法

### 手动创建Activity

创建时有两个选项

- Generate Layout File 表示会自动为建立的activity创建一个对应的布局文件
- Launcher Activity 表示会自动将建立的activity设置为当前项目的主项目

### 创建和加载布局

布局即为加载出来的界面，可以通过拖放的方式活着写XML文件的方式来编辑布局

布局有很多钟，如约束布局，线性布局等

**Android程序的设计讲究逻辑和视图分离，逻辑即为Activity，视图为布局，我们所看到的即为布局，但功能的实现还是由Activity背后的逻辑完成**

### 在AndroidManifest文件中注册

所有的activity都要在AndroidManifest.xml中注册才能生效

使用android:label指定Activity中标题栏的内容

### 在Activity中使用Toast

- Toast是一个用于提醒的控件，通过makeText()创建出一个Toast对象，然后调用show显示出来

- makeText()有三个参数，第一个参数是Context，也就是Toast要求的上下文，由于Activity本身即为一个Context对象，因此可以直接传入this
- 第二个参数是Toast显示的文本内容
- 第三个参数是Toast显示的时长

### 在Activity中使用Menu

1. 在res目录下新建一个Directory文件夹，创建名为menu，在这个文件夹新建一个main的菜单文件

2. 在main.xml中输入菜单栏标识与名称

   1. <item>标签用来创建具体的某个菜单项
   2. id为唯一标识符
   3. title为菜单名

3. 在Activity中重写onCreateOptionsMenu()方法

   ```kotlin
   override fun onCreateOptionsMenu(menu: Menu?): Boolean{
   	menuInflater.inflate(R.menu.main, menu)
   	return true
   }
   ```

   1. menuInflater实质上调用了父类的getMenuInflater方法
   2. inflate()方法接收两个参数，第一个参数用于指定我们通过那个文件来创建菜单，第二个参数用于指定我们的菜单项将添加到哪一个Menu对象中，这里使用传入的参数menu
   3. 返回值true标识允许创建的菜单显示，返回false表示无法显示

4. 在Activity中重写onOptionsltemSelected()方法

   ```kotlin
   override fun onOptionsltemSelected(item: MenuItem): Boolean {
   	when (item.itemId) {
   	R.id.
   	......
   	}
   	return true
   }
   ```

### 销毁一个Activity

调用**finish()**方法即可销毁当前activity