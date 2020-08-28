使用 IDEA 进行开发

> **与QT很像**

```protobuf
1. 安装 Android Studio 之前必须安装 JDK, Android Studio 集成开发环境的运行、Android APP 的开发，必须有 JDK 的支持

2. Android SDK 是 Android Studio 的一个插件, Android SDK 是开发 Android APP 必须的工具，我们可以使用其他工具来替代 Android Studio
```





## 目录

- [文件结构](#文件结构)
- [控件与界面布局](#控件与界面布局)
  - [单击事件](#单击事件)
- [导入和添加jar包](#导入和添加jar包)
- 









```java
// Message 对象内容 字符串截取

Message msg;

String pt = msg.obj.toString().substring(msg.obj.toString().indexOf("temp")+4,msg.obj.toString().indexOf("}"));
// 取得 {temp123}   中的  123 这三个字符, 前后是两个索引, +4 是指针偏移
```





## 文件结构

> **`app/res`  是资源文件, 其中的目录有:**
>
> - **drawable** : 自定义背景图 , 图标, xml布局文件 等
> - **layout**  : 也是放界面布局文件的
> - **mipmap** : 放图标文件的
> - **values** : 布局参数相关的 xml , **app名称** . 颜色设置 等等

- **app/manifests/AndroidManifest.xml**
  - **应用程序加载之后 就会加载这个配置文件**
  
  - 索取该APP需要的权限, 网络, 文件, 等等
  
  - `<application>` 这里面设置了APP的图标信息, 状态信息, APP名称信息,等等很多
  
  - ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.test1">
       <!-- 安卓 6.0 之后需要动态授权 网络权限 -->
        <!--允许程序打开网络套接字-->
        <uses-permission android:name="android.permission.INTERNET" />
        <!--允许程序获取网络状态-->
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
    
        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme">
          
          <!-- 下面是基本的配置, activity 代表活动, 应用程序一运行 就会加载 MainActivity 这个类  -->
            <activity android:name=".MainActivity">
    
                <!-- intent-filter 是过滤器 -->
                <intent-filter>
                    <!-- action 标记为主活动 -->
                    <action android:name="android.intent.action.MAIN" />
                    <!-- category 这个活动是拥有启动图标的  -->
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
          </activity>
          
        </application>
    
    </manifest>
    ```
    
  - 



- **app/java/com.包名.com/MainActivity.java**

  - 这是第一个java 文件.

    - 如果java涉及到界面显示  **`setContentView(R.layout.activity_main);`**  这样的代码.那么与之对应的会在 **`/app/res/layout`** 里面拥有一个 `activity_main.xml` 文件

    - ```java
      // 生成
      package com.c.ces;
      
      // 依赖项目
      import androidx.appcompat.app.AppCompatActivity;
      
      import android.media.Image;
      import android.os.Bundle;
      import android.view.View;
      import android.widget.Button;
      import android.widget.ImageView;
      import android.widget.TextView;
      import android.widget.Toast;
      
      
      // 继承这个 AppCompatActivity 类, 这是最底层的框架
      public class MainActivity extends AppCompatActivity {
            private Button btn_1;   // 私有按钮变量, 初始化
         // 只是个参数 需要进行绑定 才有用处和动作, 目前与 xml 文件的 btn_1 无关, 
            //在 activity_main.xml 里面的控件
      
          private ImageView img_1;
          private TextView text_test;
        
          // 用于在应用程序启动时加载“ native-lib”库. (拥有一个cpp 文件)
          static {
              System.loadLibrary("native-lib");
          }
      
          // 获得文本编辑框的ID并赋予 tv,  简化了 findViewByd()
      		@BindView(R.id.sample_text)
      	  public TextView tv ;
      
        
          @Override
          protected void onCreate(Bundle savedInstanceState) {
            // 这里是 界面打开后, 最先运行的地方
              super.onCreate(savedInstanceState);
      
            //  一般先用来进行界面初始化控件初始化初始化一些参数和变量 
            //    不恰当比方类似于单片机的 main 函数 
            // 对应的文件就是 app/res/layout/activity_main.xml 文件, 载入这个界面
            //  R 表示的是资源类  res 目录下的内容
              setContentView(R.layout.activity_main);
      			
              // 调用本机方法的示例, findViewById() 寻找界面布局 android:id="@+id/sample_text"
              // 对应的文件就是 app/res/layout/sample_text.xml 文件
              TextView tv = findViewById(R.id.sample_text);
       	      ui_init();
              tv.setText(stringFromJNI());
      
          }
      
          /**
           * A native method that is implemented by the 'native-lib' native library,
         * which is packaged with this application.
           */
          public native String stringFromJNI();
        
        
          private  void ui_init(){
              btn_1 = findViewById(R.id.btn_1);  //寻找xml 里面真正的id, 然后与自己定义的id绑定, 这样才有意义
              img_1 = findViewById(R.id.img_1);
              text_test = findViewById(R.id.text_test);
      
              // 设置 在 (参数) 侦听器上监听 这个按钮的单击事件,  View.OnClickListener() 新的点击监听器
              btn_1.setOnClickListener(new View.OnClickListener() {
                  @Override
                  public void onClick(View v) {
                      // 这里是单击之后就执行的地方, 和 lambda 表达式很像, 作用相同
      
                      //在 调试终端输出
                      System.out.println("hellp test info \n");
      
                      // 悬浮提示窗口 ( MainActivity.this 当前的界面, "弹窗显示的内容" , Toast.LENGTH_SHORT 显示的时长).显示();
                      // 1秒后悬浮窗就会自己消失
                      Toast.makeText(MainActivity.this, "hello msg", Toast.LENGTH_SHORT).show();
                  }
              });
      
      
              img_1.setOnClickListener(new View.OnClickListener() {
                  @Override
                  public void onClick(View view) {
                      Toast.makeText(MainActivity.this, "第一个图片", Toast.LENGTH_SHORT).show();
                      text_test.setText("新内容");  // 设置另一个对象的内容
                  }
              });
          }
      }
      
      ```
      
    - 





## 控件与界面布局

> **界面布局文件存在于   `app/res/layout` 目录下, 都是.xml 文件**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

<!--  LinearLayout 线性布局模式，一排一排的 , 当成容器看 -->
<!--  match_parent 表示充满父控件, wrap_content 自适应长宽 -->
<!-- android:id="@+id/linearLayout"  设置 控件ID, 在文件中唯一用来和Java文件通信,或者绑定事件  -->
<!-- android:orientation="vertical" 设置布局为 纵向布局, 默认横向 -->
<!-- android:gravity="center"  设置布局内的内容控件 全部居中, 内部权重 -->
<!--  android:layout_margin="10dp" 左右两边边距距离  -->
<!--  android:layout_weight="1"   设置权重, 让当前控件占据更多控件, 相同时则均分.数字越大 占的位置越多 -->
<!-- android:layout_gravity="center_horizontal"  以于父控件作为参考,设置自己控件的位置, 让控件水平居中-->
<!-- android:gravity="bottom"  以本身作为参考, 设置控件内的内容显示位置 -->
  
    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="@android:dimen/thumbnail_width"
            android:src="@drawable/p"
            android:layout_gravity="center" />


        <!--  TextView 显示文本 -->
<!-- android:layout_gravity="center_horizontal"  以于父控件作为参考,设置自己控件的位置, 让控件水平居中-->
        <TextView
            android:id="@+id/text_1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="hello"
            android:textSize="@android:dimen/app_icon_size" />


<!--  Button 按钮, 90dp是长度单位 , #01F432 是颜色, RGB -->
<!--  @mipmap/ic_launcher_round, 中的 @ 指的是 res 目录下的内容 -->
        <Button
            android:id="@+id/bt_1"
            android:layout_width="120dp"
            android:layout_height="90dp"

            android:background="@drawable/kaiguanguan"
            android:text="确定"
            android:textColor="#01F432"

            android:textSize="30sp" />

<!--   文本编辑框    -->
        <EditText
            android:id="@+id/edittext"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:hint="输入一些东西" />

<!-- 显示图片资源 , 使用 src 可以实现更换图片的功能, background这不行 -->
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@drawable/kaiguanguan" />

    </LinearLayout>


</androidx.constraintlayout.widget.ConstraintLayout>
```



### 单击事件

**测试**

```xml

<?xml version="1.0" encoding="utf-8"?>


<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"


    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

<!--  LinearLayout 线性布局模式，一排一排的 -->
<!--  match_parent 表示充满父控件, wrap_content 自适应长宽 -->
<!-- android:id="@+id/linearLayout"  设置 控件ID, 在文件中唯一用来和Java文件通信,或者绑定事件  -->
<!-- android:orientation="vertical" 设置布局为 纵向布局, 默认横向 -->
<!-- android:gravity="center"  设置布局内的内容控件 全部居中, 内部权重 -->
    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#FFCCCC"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="10dp"
        tools:layout_editor_absoluteY="10dp">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="233dp">

            <!--  android:layout_margin="10dp" 左右两边边距距离  -->
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_margin="10dp"
                android:src="@drawable/p" />

        </LinearLayout>

<!--  android:layout_weight="1"   设置权重, 让当前控件占据更多控件, 相同时则均分 -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="20dp"
            android:orientation="vertical">
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content">
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:layout_weight="1"
                    android:gravity="center">

                    <ImageView
                        android:layout_width="100dp"
                        android:layout_height="60dp"
                        android:src="@drawable/kaiguanguan" />
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_gravity="center_horizontal"
                        android:text="商品1"
                        android:textSize="20sp"
                        />
                </LinearLayout>
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:layout_weight="1"
                    android:gravity="center">

                    <ImageView
                        android:layout_width="100dp"
                        android:layout_height="60dp"
                        android:src="@drawable/kaiguanguan" />
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_gravity="center_horizontal"
                        android:text="商品2"
                        android:textSize="20sp"
                        />
                </LinearLayout>
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:layout_weight="1"
                    android:gravity="center">

                    <ImageView
                        android:layout_width="100dp"
                        android:layout_height="60dp"
                        android:src="@drawable/kaiguanguan" />
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_gravity="center_horizontal"
                        android:text="商品3"
                        android:textSize="20sp"
                        />
                </LinearLayout>


            </LinearLayout>
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="20dp"
                ><LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:layout_weight="1"
                android:gravity="center">

                <ImageView
                    android:layout_width="100dp"
                    android:layout_height="60dp"
                    android:src="@drawable/kaiguanguan" />
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="center_horizontal"
                    android:text="商品4"
                    android:textSize="20sp"
                    />
            </LinearLayout>
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:layout_weight="1"
                    android:gravity="center">

                    <ImageView
                        android:layout_width="100dp"
                        android:layout_height="60dp"
                        android:src="@drawable/kaiguanguan" />
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_gravity="center_horizontal"
                        android:text="商品5"
                        android:textSize="20sp"
                        />
                </LinearLayout>
                <LinearLayout
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:layout_weight="1"
                    android:gravity="center">

                    <ImageView
                        android:layout_width="100dp"
                        android:layout_height="60dp"
                        android:src="@drawable/kaiguanguan" />
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_gravity="center_horizontal"
                        android:text="商品6"
                        android:textSize="20sp"
                        />
                </LinearLayout>
            </LinearLayout>
        </LinearLayout>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="测试按钮"
            android:id="@+id/btn_1"/>
    </LinearLayout>


</androidx.constraintlayout.widget.ConstraintLayout>
```



## 导入和添加jar包

> 1. 首先下载需要的 **jar** 包.  例如下载的是  **org.jar**
> 2. 打开项目工程目录  **`工程名/app/libs`** 
> 3. 将 **jar** 拷贝到这目录
> 4. 然后右键这个已经拷贝过来的 **jar** 包, 找到 **Add As Library..** 这个选项, 单击选择
> 5. 查看 `工程名/app/build.grable`  这个文件内. 是否存在 org.jar 这个包名和路径, 存在则表示完成

















