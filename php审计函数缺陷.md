## **参数处理函数缺陷**
#### **1、urldecode — 解码已编码的 URL 字符串**
```
语法：urldecode( string $str) : string
解码给出的已编码字符串中的任何 %##。 加号（'+'）被解码成一个空格字符。

下面列举了一个关于urldecode()引发SQL注入的例子（具体漏洞编号没找到）
是以前Discuz X1.5的一个漏洞
foreach($_POST as $k => $v) {
$value = urldecode($v);
$this->setParameter($k, $value);
}

单引号被 urlencode 两次以后是 %2527，然后 POST，PHP 内部在生成全局变量 $_POST 的时候会先 urldecode，得到 %27，
然后 PHP 会检查 Magic Quotes 的设置，但是无论是否开启 Magic Quotes，
%27 都不会被转义，因为这时根本没有单引号。但是这时如果在PHP中加上 urldecode，%27就变成单引号了
```

^
#### **2、rawurldecode — 对已编码的 URL 字符串进行解码**
```
语法 rawurldecode( string $str) : string
返回字符串，此字符串中百分号（%）后跟两位十六进制数的序列都将被替换成原义字符。
这个就不细讲了，和urldecode差不多。
```

^
#### **3、is_numeric**
```
bool is_numeric ( mixed $var )
如果 var 是数字和数字字符串则返回 TRUE，否则返回 FALSE。
仅用is_numeric判断而不用intval转换就有可能插入16进制的字符串到数据库，进而可能导致sql二次注入。
```


^
#### **4、in_array()没有true**
用in_array()做白名单，但是第三个参数没有用true。
```
例如文件名为 7shell.php 。
因为PHP在使用 in_array()函数判断时，
会将 7shell.php 强制转换成数字7，而数字7在 range(1,24)数组中，
最终绕过 in_array() 函数判断，导致任意文件上传漏洞。
sql注入等。
```
