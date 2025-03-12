

## **NDK开发**
NDK（Native Development Kit）是一套用于开发Android应用程序的工具集，它允许您在C/C++中编写性能关键的部分代码，并将这些代码与Java代码进行连接。

^


1.AS下载NDK和CMake
![](.topwrite/assets/image_1741756441671.png)

2.AS新建一个项目,往下拉,找到"c++"这个选项



3.查看src/main/cpp/ CMakeLists.txt和编写native-lib.cpp 下面是cmakelist.txt和native-lib.cpp文件的作用以及简要说明：

| 文件名            | 作用       | 说明                                                                                                     |
| -------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| CMakeLists.txt | 构建配置文件   | CMakeLists.txt是用于配置NDK项目的构建系统的文件。它指定了构建所需的源文件、依赖项、编译选项等。在构建过程中，CMake会根据该文件的指示生成对应的构建脚本，用于编译本地代码并生成本地库。 |
| native-lib.cpp | 本地代码实现文件 | native-lib.cpp是包含本地代码实现的文件。它定义了通过Java和本地代码之间进行通信的本地方法。该文件中的函数实现将被编译为本地库，供Java代码调用。                     |




解释：

```java
public class MainActivity extends AppCompatActivity {

    // Used to load the 'ndkdemo' library on application startup.
    static {
        System.loadLibrary("ndkdemo"); // 加载名为"ndkdemo"的库
    }

    private ActivityMainBinding binding; // 声明一个ActivityMainBinding变量

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding = ActivityMainBinding.inflate(getLayoutInflater()); // 使用ViewBinding将布局文件解析为一个ActivityMainBinding对象
        setContentView(binding.getRoot()); // 将Activity的布局设置为根布局

        // Example of a call to a native method
        TextView tv = binding.sampleText; // 获取布局文件中的TextView控件
        tv.setText(stringFromJNI()); // 调用本地方法stringFromJNI()并将其返回的字符串设置为TextView的文本内容
    }

    /**
     * A native method that is implemented by the 'ndkdemo' native library,
     * which is packaged with this application.
     */
    public native String stringFromJNI(); // 声明一个native方法stringFromJNI()
}

```

```java
# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# 设置构建本地库所需的CMake的最小版本要求
cmake_minimum_required(VERSION 3.22.1)

# 声明和命名项目
project("ndkdemo")

# 创建并命名一个库，设置其类型为STATIC或SHARED，并指定源代码的相对路径
# 可以定义多个库，CMake会为您构建它们
# Gradle会自动将共享库打包到APK中
add_library(
        # 设置库的名称
        ndkdemo

        # 设置库类型为共享库
        SHARED

        # 提供源文件的相对路径
        native-lib.cpp)

# 搜索指定的预构建库并将路径存储为变量。
# 由于CMake默认在搜索路径中包含系统库，因此您只需指定要添加的公共NDK库的名称。
# CMake会在完成构建之前验证该库是否存在。
find_library(
        # 设置路径变量的名称
        log-lib

        # 指定要让CMake定位的NDK库的名称
        log)

# 指定CMake应链接到目标库的库。
# 您可以链接多个库，例如在此构建脚本中定义的库、预构建的第三方库或系统库。
target_link_libraries(
        # 指定目标库
        ndkdemo

        # 将目标库链接到NDK中包含的log库
        ${log-lib})

```

```c++
#include <jni.h> // JNI头文件，提供了JNI函数和数据类型的定义
#include <string> // C++标准库的string类

// 声明一个jni函数，该函数将会被Java代码调用
// JNIEXPORT表示这个函数是可导出的，并且可以被其他代码使用
// jstring表示这个函数返回的是一个Java字符串对象
// JNICALL是JNI函数的调用约定
// Java_com_example_ndkdemo_MainActivity_stringFromJNI是JNI函数的命名规则，与Java中对应的方法名对应
// Java打头，1包名,2类名,3方法名字;"_"号隔开
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_ndkdemo_MainActivity_stringFromJNI(
        JNIEnv* env, // JNIEnv是指向JNI环境的指针，可以用来访问JNI提供的功能
        jobject /* this */) { // jobject是指向Java对象的指针，在本例中并没有使用

    std::string hello = "Hello from C++"; // 创建一个C++字符串对象
    return env->NewStringUTF(hello.c_str()); // 将C++字符串对象转换为Java字符串对象并返回
}


```


