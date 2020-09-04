## 目录

- [项目中cpp内的cmake文件](#项目中cpp内的cmake文件)
- [app目录内的build-gradle文件](#app目录内的build-gradle文件)
- 







## 项目中cpp内的cmake文件

```cmake

# 那个项目的 cmake 文件, 主要区分包含和语法
cmake_minimum_required(VERSION 3.4.1)

add_library( #生成的文件名( 不会生成, 就是个标志)
					    server-lib
						#生成 .lib文件
             SHARED
            # 文件和具体路径
        CELL.hpp base64.hpp base64.cpp errnoRet.hpp errnoRet.cpp
        InitNetWork.hpp InitNetWork.cpp onRun.hpp onRun.cpp server.cpp )

add_library( # Sets the name of the library.
        mod-lib

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        MemoryTools.hpp  mod-lib.cpp)

#找到 android的NDK, 默认自带的
find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

#每个 lib文件都需要独自拥有一个 target_link_libraries 项目
target_link_libraries( # Specifies the target library.
        server-lib

        # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
target_link_libraries( 
        mod-lib
        ${log-lib} )

```





## app目录内的build-gradle文件

```groovy
apply plugin: 'com.android.application'

android {
    compileSdkVersion 29
    buildToolsVersion '29.0.2'

    defaultConfig {
        applicationId "com.tencent.eac"
        minSdkVersion 22
        targetSdkVersion 29
        versionCode 2
        versionName "1.2"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        externalNativeBuild {
            cmake {
                cppFlags ""
            }
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    buildTypes {
        release {
            //minifyEnabled false

            // 不显示Log
            buildConfigField "boolean", "LOG_DEBUG", "false"
            //混淆
            minifyEnabled true
            //Zipalign优化
            zipAlignEnabled true
            // 移除无用的resource文件
            shrinkResources true
            //加载混淆配置文件
            //proguard-android.txt为默认的混淆文件
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    externalNativeBuild {
        cmake {
            path "src/main/cpp/CMakeLists.txt"
            version "3.10.2"
        }
    }
    ndkVersion '21.3.6528147'
}

dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.0'

    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
  
  // 在 app/libs 目录内添加的两个java库
    implementation files('libs\\fastjson-1.2.9.jar')
    implementation files('libs\\okio-1.9.0.jar')
  
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'

    // butterknife
    implementation 'com.jakewharton:butterknife:10.0.0'
    annotationProcessor 'com.jakewharton:butterknife-compiler:10.0.0'

    implementation 'com.android.support:cardview-v7:29.0.0'
}
```









