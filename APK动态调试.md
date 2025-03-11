## **2.什么是动态调试**

动态调试是指自带的调试器跟踪自己软件的运行，可以在调试的过程中知道参数或者局部变量的值以及履清代码运行的先后顺序。多用于爆破注册码(CTF必备技能)。

注册码是根据机型特征特定算法生成的存储在临时寄存器中。

## **3.动态调试步骤**

### 1.修改debug权限
方法一:在AndroidManifest.xml里添加可调试权限
```xml
android:debuggable="true"
```

方法二：XappDebug模块hook对应的app
项目地址[XappDebug](https://github.com/Palatis/XAppDebug)
记得重启即可开启debug

方法三：Magisk命令(重启失效)

```
1.  adb shell #adb进入命令行模式
    
2.  su #切换至超级用户
    
3.  magisk resetprop ro.debuggable 1
    
4.  stop;start; #一定要通过该方式重启
```

方法四:刷入MagiskHide Props Config模块(永久有效，但我这两台手机都不行)

一般来说，在4选项中如果有ro.debuggable那就直接修改
没有的话就选5
![](.topwrite/assets/image_1741613897044.png)
修改ro.debuggable的值为1


### 2.端口转发以及开启adb权限


版本号点击七次开启开发者模式并开启adb调试权限

夜神模拟器：adb connect 127.0.0.1:62001

### 3.jeb下断点
jeb中
ctrl+b下断点 

### 4.debug模式启动


```
adb shell am start -D -n com.zj.wuaipojie/.ui.MainActivity
```
adb shell am start -D -n
adb shell am start -D -n 包名/类名
am start -n 表示启动一个activity
am start -D 表示将应用设置为可调试模式

### 5.Jeb附加调试进程
**激活jeb：**
<https://www.52pojie.cn/thread-1598242-1-1.html>

^
### **jeb使用**
jeb.bat启动，将安装包apk拖入。

底部快捷字符搜索，字符常量。
字符常量，右键解析，跳转java代码。
java代码，双击进入函数。

快捷键：
F6进入方法
F7从方法中跳出来
F8
R运行到光标处

下断点：
ctrl+b

## **3.Log插桩**

定义：Log插桩指的是反编译APK文件时，在对应的smali文件里，添加相应的smali代码，将程序中的关键信息，以log日志的形式进行输出。

调用命令
```smali

invoke-static {对应寄存器}, Lcom/mtools/LogUtils;->v(Ljava/lang/Object;)V

```

