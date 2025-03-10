## 2.什么是动态调试

动态调试是指自带的调试器跟踪自己软件的运行，可以在调试的过程中知道参数或者局部变量的值以及履清代码运行的先后顺序。多用于爆破注册码(CTF必备技能)

## 3.动态调试步骤

### 1.修改debug权限
方法一:在AndroidManifest.xml里添加可调试权限
```xml
android:debuggable="true"
```

方法二：XappDebug模块hook对应的app

项目地址

[XappDebug](https://github.com/Palatis/XAppDebug)

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

### 3.下段点


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

激活jeb

在线python运行
[https://tool.lu/coderunner/](https://tool.lu/coderunner/)

算号代码：
```python
#[url=https://bbs.pediy.com/]https://bbs.pediy.com/[/url]
#!/usr/bin/env python
import os, sys, struct, time, binascii, hashlib
 
RC4_Key2= 'Eg\xa2\x99_\x83\xf1\x10'
 
def rc4(Key, inData):
    Buf = ""
    S = range(256)
    K = (map(lambda x:ord(x), Key) * (256 / len(Key) + 1))[:256]
    j = 0
    for i in range(256):
        j = (S[i] + K[i] + j) % 256
        S[i], S[j] = S[j], S[i]
    i, j = 0, 0
    for x in range(len(inData)):
        i = (i + 1) % 256
        j = (j + S[i]) % 256
        S[i], S[j] = S[j], S[i]
        Buf += chr(S[(S[j] + S[i]) % 256] ^ ord(inData[x]))
    return Buf
 
def Long2Int(longdata):
    lo = longdata & 0xFFFFFFFF
    hi = (longdata >> 32) & 0x7FFFFFFF
    return hi, lo
 
def KeygenSN(LicenseSerial, MachineID):
    mhi, mlo = Long2Int(MachineID)
    lhi, llo = Long2Int(LicenseSerial)
    hi_Key = (mhi - lhi + 0x55667788) & 0x7FFFFFFF
    lo_Key = (mlo + llo + 0x11223344) & 0xFFFFFFFF
    Z0, = struct.unpack('<Q', struct.pack('<LL', lo_Key, hi_Key))
    Z1 = int(time.time()) ^ 0x56739ACD
    s = sum(map(lambda x:int(x, 16), "%x" % Z1)) % 10
    return "%dZ%d%d" % (Z0, Z1, s)
 
def ParsePost(buf):
    Info = struct.unpack('<3L2Q4LQ3L', buf[:0x40])
    flag, CRC, UserSerial, LicenseSerial, MachineID, build_type, \
          Ver_Major, Ver_Minor, Ver_Buildid, Ver_Timestamp, \
          TimeOffset, Kclass, Random2 = Info
    SysInfoData = buf[0x40:]
    assert CRC == binascii.crc32(buf[8:]) & 0xFFFFFFFF
    return Info, SysInfoData
 
def DecodeRc4Str(buf):
    buf = buf.decode('hex')
    i, s = ParsePost(rc4(buf[:8] + RC4_Key2, buf[8:]))
    return i, s
 
def GetJebLicenseKey():
    licdata = ""
    if licdata:
        i, MachineID = DecodeRc4Str(licdata)
        SN = KeygenSN(i[3], i[4])
        print "JEB License Key:", SN
        return SN
 
GetJebLicenseKey()
raw_input("Enter to Exit...")

```

快捷键：
F6进入方法
F7从方法中跳出来
F8
R运行到光标处

## 3.Log插桩

定义：Log插桩指的是反编译APK文件时，在对应的smali文件里，添加相应的smali代码，将程序中的关键信息，以log日志的形式进行输出。

调用命令
```smali

invoke-static {对应寄存器}, Lcom/mtools/LogUtils;->v(Ljava/lang/Object;)V

```

