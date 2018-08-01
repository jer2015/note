### linux平台JAVA调用so库-

#### 安装gcc编译器

#### 编写java代码,native声明需要c实现的函数

```
package cn.tom
public class HelloJNI
{
    static
    {
        System.loadLibrary("goodLuck"); 
        //放在java.library.path目录下 System.getProperty("java.library.path")
        // System.load("/cn/tom/goodLuck.so"); 绝对路径
    }
    public native static int get();
    public native static void set(int i);
}
```

#### 编译java文件

```
javac HelloJNI.java
```

#### 根据编译生成的.class文件生成.h头文件

```
javah -classpath ~/IdeaProjects/myDev/springdemo/springdemo/src/test/java -d . -jni cn.tom.HelloJNI
```

> -classpath 包的根路径
>
> -d 生成文件位置

#### 编写HelloJNI.c文件,引用.h头文件

```
#include "cn_tom_HelloJNI.h"

int i = 0;
JNIEXPORT jint JNICALL Java_cn_tom_HelloJNI_get(JNIEnv *env, jclass jc)
{
        return i;
}
JNIEXPORT void JNICALL Java_cn_tom_HelloJNI_set(JNIEnv *env, jclass jc, jint j)
{
        i = j;
}
```

#### 根据.c文件编译生成.o文件

```
gcc -fPIC -D_REENTRANT -I/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home/include -I/Library/Java/JavaVirtualMachines/jdk1.8.0_101.jdk/Contents/Home/include/darwin -c HelloJNI.c
```

#### 将生成的.o文件生成.so文件

```
gcc -shared HelloJNI.o -o libgoodLuck.so
```

