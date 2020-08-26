- [新建一个界面](#新建一个界面)
- [跳转和添加数据](#跳转和添加数据)
- 



> **Intent (意图):   将要执行的动作抽象的描述, 由Intent 来协助完成 Android 各个组件之间的通讯.**



- **使用 `Intent`  传送数据:**
  - **添加数据:  `Intent.putExtra( key, value);`**
  - **获取数据: `Intent.getTypeExtra( key);`**
  - **比较复杂的引用数据类型(自定义java对象):  需要进行序列化( 将一个对象转换成可存储或可传输的状态, 实现 `Serializable` 接口是一种序列化方式)  再进行传输** 



## 新建一个界面

> - **首先在 app/java/com.xxx  目录下新建一个 class 文件**
>
>   - **然后添加成为如下样子**
>
>     - ```java
>       package com.example.app03;  // 包名.
>       
>       import android.os.Bundle;
>       
>       import androidx.annotation.Nullable;
>       import androidx.appcompat.app.AppCompatActivity;
>       
>       // FirstActivity是文件名和类名, extends AppCompatActivity  代表继承基础类
>       public class FirstActivity extends AppCompatActivity {
>         // onCreate() 重写, 必须是这个方法
>           @Override
>           protected void onCreate(@Nullable Bundle savedInstanceState) {
>               super.onCreate(savedInstanceState);
>             
>                // 设置他说绑定的 xml界面布局配置文件 activity_first.xml , 需要新建
>               setContentView(R.layout.activity_first);
>           }
>       }
>       
>       ```
>
> - **然后在 app/layout  创建一个 activity_first .xml文件,在里面设计界面即可**
>
> - **最后在  app/manifests/AndroidManifest.xml  里面修改成如下内容:**
>
>   - ```xml
>     <?xml version="1.0" encoding="utf-8"?>
>     <manifest xmlns:android="http://schemas.android.com/apk/res/android"
>         package="com.example.app03">
>     
>         <application
>             android:allowBackup="true"
>             android:icon="@mipmap/ic_launcher"
>             android:label="@string/app_name"
>             android:roundIcon="@mipmap/ic_launcher_round"
>             android:supportsRtl="true"
>             android:theme="@style/AppTheme">
>             <activity android:name=".MainActivity">
>                 <intent-filter>
>                     <!--  过滤条件, 让当前界面成为程序运行后第一个出现的界面 -->
>                     <action android:name="android.intent.action.MAIN" />
>                     <!-- 在桌面上 显示系统图标 -->
>                     <category android:name="android.intent.category.LAUNCHER" />
>                 </intent-filter>
>             </activity>
>     
>     <!-- 当添加行新界面时, 就要在这里声明那个新界面的内容 -->
>             <activity android:name=".FirstActivity" />
>         </application>
>     
>     </manifest>
>     ```
>
>   - 



## 跳转和添加数据

### MainActivity.java

```java
package com.example.app03;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.example.app03.entity.User;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    // final 等同于 const , 不可修改变量,不可修改变量的指向  =  const char* const s;
    // 标志着, 这个日志是由 MainActivity 类打印的
    private static final String TAG = "MainActivity";

    //    @Override 关键字 代表 重写
    // 启动后第一个执行的方法
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //初始化方法
        initUI();
    }

    private void initUI() {
        // 点击事件, 重写onClick() 方法, 设置监听者对象
        // 下面俩个 都会调用同一个方法  就是 onClick()
        findViewById(R.id.btn1).setOnClickListener(this);
        findViewById(R.id.btn2).setOnClickListener(this);
    }


    @Override
    public void onClick(View view) {
        Intent intent = new Intent();  // 得到Intent 对象

        // 添加数据，传递给下一个界面.  name是键值, 李明是 value
        intent.putExtra("name", "李明");

        //创建一个新对象 User 类, 自定义的类,在 entity/User 中
        User user = new User();
        user.setId(123);
        user.setName("张三");

        // 传递一个自定义 序列化的类 . Serializable, 然后才可以传递
        intent.putExtra("user", user);

        // view.getId() 获得调用该方法的 控件ID
        switch (view.getId()){
            case R.id.btn1:{
                // 跳转到第一个界面

                // 设置一个 意图, 从当前界面 跳转到另一个页面 (FirstActivity)
                // getApplicationContext() 获得上下文环境
                //  FirstActivity.class 指定字节码文件,  FirstActivity是那个自定义的java文件
                intent.setClass(getApplicationContext(), FirstActivity.class);
            }break;
            case R.id.btn2:{
                // 跳转到第二个界面
                intent.setClass(getApplicationContext(), SecondActivity.class);
            }break;
        }

        //跳转到新的 界面(intent)
        this.startActivity(intent);
    }
}
```



### FirstActivity.java

```java
package com.example.app03;

import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import com.example.app03.entity.User;

public class FirstActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 设置成绑定的 xml界面布局配置文件 activity_first.xml
        setContentView(R.layout.activity_first);


        // 获得上个页面传递过来的数据集合
        Bundle bundle =  getIntent().getExtras();

        //通过键值 "name" 来找到传递过来的字符串 , bundle.getInt() 获得int值
        String str =  bundle.getString("name");
        Log.i("name: ",str);


        // 获取序列化之后的自定义类 的数据, 需要转换
        User  user = (User) bundle.getSerializable("user");
        Log.i("User.getName:", user.getName());

        TextView tvShow =  findViewById(R.id.tv_show);
        tvShow.setText(str);
        tvShow.setText("" + user.getId());
    }
}
```



### SecondActivity.java

```java
package com.example.app03;

import android.os.Bundle;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);
    }
}

```



### AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.app03">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <!--  过滤条件, 让当前界面成为程序运行后第一个出现的界面 -->
                <action android:name="android.intent.action.MAIN" />
                <!-- 在桌面上 显示系统图标 -->
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

<!-- 当添加行新界面时, 就要在这里声明那个新界面的内容 -->
        <activity android:name=".FirstActivity"  />
        <activity android:name=".SecondActivity" />
    </application>

</manifest>
```



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>

<!--LinearLayout 线性布局-->
<!--android:orientation="vertical" 竖向布局-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">


    <Button
        android:id="@+id/btn1"
        android:text="按钮1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

    <Button
        android:id="@+id/btn2"
        android:text="按钮2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

</LinearLayout>
```



### activity_first.xml

```xml
<?xml version="1.0" encoding="utf-8"?>

<!--LinearLayout 线性布局-->
<!--android:orientation="vertical" 竖向布局-->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">


    <Button
        android:id="@+id/btn1"
        android:text="按钮1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

    <Button
        android:id="@+id/btn2"
        android:text="按钮2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

</LinearLayout>
```



### activity_second.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:text="这是第二个界面"
        android:textSize="25dp"
        android:textColor="@android:color/holo_green_dark"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
</LinearLayout>
```









