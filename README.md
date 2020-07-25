使用 IDEA 进行开发

## 目录

- [文件结构](#文件结构)
- [控件与界面布局](#控件与界面布局)
- 
- 











## 文件结构

> `app/res`  是资源文件, 其中的目录有:
>
> - **drawable** : 自定义背景图 , 图标, xml布局文件 等
> - **layout**  : 也是放界面布局文件的
> - **mipmap** : 放图标文件的
> - **values** : 布局参数相关的 xml , **app名称** . 颜色设置 等等

- **app/manifests/AndroidManifest.xml**
  - 索取该APP需要的权限, 网络, 文件, 等等
  
  - `<application>` 这里面设置了APP的图标信息, 状态信息, APP名称信息,等等很多
  
  - ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.test1">
    
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
            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
    
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
      import android.os.Bundle;
      import android.widget.TextView;
      
      // 继承这个 AppCompatActivity 类, 这是最底层的框架
      public class MainActivity extends AppCompatActivity {
        
          // 用于在应用程序启动时加载“ native-lib”库. (拥有一个cpp 文件)
          static {
              System.loadLibrary("native-lib");
          }
      
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
              tv.setText(stringFromJNI());
          }
      
          /**
           * A native method that is implemented by the 'native-lib' native library,
         * which is packaged with this application.
           */
          public native String stringFromJNI();
      }
      
      ```
      
    - 





## 控件与界面布局

> **界面布局文件存在于   `app/res/layout` 目录下, 都是.xml 文件**





















