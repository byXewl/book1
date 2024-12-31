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


## **原型链继承**


所有类对象在实例化的时候将会拥有`prototype`中的属性和方法，这个特性被用来实现JavaScript中的继承机制。

比如：

```
function Father() {
    this.first_name = 'Donald'
    this.last_name = 'Trump'
}

function Son() {
    this.first_name = 'Melania'
}

Son.prototype = new Father()

let son = new Son()
console.log(`Name: ${son.first_name} ${son.last_name}`)
```

Son类继承了Father类的`last_name`属性，最后输出的是`Name: Melania Trump`。

总结一下，对于对象son，在调用`son.last_name`的时候，实际上JavaScript引擎会进行如下操作：

1. 在对象son中寻找last\_name
2. 如果找不到，则在`son.__proto__`中寻找last\_name
3. 如果仍然找不到，则继续在`son.__proto__.__proto__`中寻找last\_name
4. 依次寻找，直到找到`null`结束。比如，`Object.prototype`的`__proto__`就是`null`



JavaScript的这个查找的机制，被运用在面向对象的继承中，被称作prototype继承链。

