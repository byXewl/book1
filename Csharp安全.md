

### **基础知识：**

ASP.NET开发可以选用两种框架：`ASP.NET Core`与`ASP.NET Framework`

ASP.NET开发也分为两种：
1、WebApplication：
```
WEB应用程序，改变代码后需要重启网页。具有namespace空间名称，项目中所有的程序代码文件，和独立的文件都被编译成为一个程序集，保存在bin文件夹中。
```
2、WebSite：
```
WEB网站，改变代码后不用重启网页。它没用到namespace空间名称，每个asp页面会转成一个dll。
```

^
## **ASP.NET比较关键的文件：**
> web.config:

```
1.web.config是基于XML的文件，可以保存到Web应用程序中的任何目录中，
用来储存数据库连接字符、身份安全验证等。
2.加载方式：当前目录搜索 -> 上一级到根目录 ->
 %windir%/Microsoft.NET/Framework/v2.0.50727/CONFIG/web.config ->
 %windir%/Microsoft.NET/Framework/v2.0.50727/CONFIG/machine.config -> 都不存在返回null
```

> Global.asax：

```
1. Global.asax提供全局可用的代码，从HttpApplication基类派生的类，响应的是应用程序级别会话级别事件，通常ASP.NET的全局过滤代码就是在这里面。
```

^
## **ASP.NET的常见拓展名：**

在`%windir%\Microsoft.NET\Framework\v2.0.50727\CONFIG\web.config`中有详细定义，这里提取部分简单介绍。


```
aspx：应用程序根目录或子目录，包含web控件与其他
cs：类文件
aspx.cs：web窗体后台程序代码文件
ascx：应用程序根目录或子目录,Web 用户控件文件。
asmx：应用程序根目录或子目录，该文件包含通过 SOAP 方式可用于其他 Web 应用程序的类和方法。
asax：应用程序根目录，通常是Global.asax
config：应用程序根目录或子目录，通常是web.config
ashx：应用程序根目录或子目录,该文件包含实现 IHttpHandler 接口以处理所有传入请求的代码。
soap：应用程序根目录或子目录。soap拓展文件
```




^
## **代码审计**
<https://www.geekby.site/2024/08/dotnet%E4%BB%A3%E7%A0%81%E5%AE%A1%E8%AE%A1%E7%B3%BB%E5%88%97%E4%B8%80/>


^
## **代码审计实战思路**
1、熟悉框架目录