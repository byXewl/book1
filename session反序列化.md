前置知识：
```
ini_set('session.serialize_handler', 'php') 时的Session文件内容例：user|s:3:"xxx";

ini_set('session.serialize_handler', 'php_serialize') 时的Session文件内容例：a:1:{s:4:"user";s:3:"xxx";}
```