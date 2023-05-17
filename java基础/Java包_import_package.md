Java包

包主要用来对类和接口进行分类。当开发Java程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。

## Import语句

在Java中，如果给出一个完整的限定名，包括包名、类名，那么Java编译器就可以很容易地定位到源代码或者类。Import语句就是用来提供一个合理的路径，使得编译器可以找到某个类。

```java
例如，下面的命令行将会命令编译器载入java_installation/java/io路径下的所有类
import java.io.*;
```

**一个完整的Java。源程序应该包括下列部分：**

-  **package语句，该部分至多只有一句，必须放在源程序的第一句。**
-  **import语句，该部分可以有若干import语句或者没有，必须放在所有的类定义之前。**
-  **public classDefinition，公共类定义部分，至多只有一个公共类的定义，Java语言规定该Java源程序的文件名必须与该公共类名完全一致。**
-  **classDefinition，类定义部分，可以有0个或者多个类定义。** 
- **interfaceDefinition，接口定义部分，可以有0个或者多个接口定义。**

例如：

```java
package javawork.helloworld;
/*把编译生成的所有．class文件放到包javawork.helloworld中*/
import java awt.*;
//告诉编译器本程序中用到系统的AWT包
import javawork.newcentury;
/*告诉编译器本程序中用到用户自定义的包javawork.newcentury*/
 public class HelloWorldApp{...｝
/*公共类HelloWorldApp的定义，名字与文件名相同*/ 
class TheFirstClass｛...｝;
//第一个普通类TheFirstClass的定义 
interface TheFirstInterface{......}
/*定义一个接口TheFirstInterface*/
```

**package语句：**由于Java编译器为每个类生成一个字节码文件，且文件名与类名相同因此同名的类有可能发生冲突。为了解决这一问题，Java提供包来管理类名空间，包实 提供了一种命名机制和可见性限制机制。