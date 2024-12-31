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

以上就是最基础的JavaScript面向对象编程，我们并不深入研究更细节的内容，只要牢记以下几点即可：

1. 每个构造函数(constructor)都有一个原型对象(prototype)
2. 对象的`__proto__`属性，指向类的原型对象`prototype`
3. JavaScript使用prototype链实现继承机制


^
## **原型链污染**


第一章中说到，`foo.__proto__`指向的是`Foo`类的`prototype`。那么，如果我们修改了`foo.__proto__`中的值，是不是就可以修改Foo类呢？

做个简单的实验：

```
// foo是一个简单的JavaScript对象
let foo = {bar: 1}

// foo.bar 此时为1
console.log(foo.bar)

// 修改foo的原型（即Object）
foo.__proto__.bar = 2

// 由于查找顺序的原因，foo.bar仍然是1
console.log(foo.bar)

// 此时再用Object创建一个空的zoo对象
let zoo = {}

// 查看zoo.bar
console.log(zoo.bar)
```

最后，虽然zoo是一个**空**对象`{}`，但`zoo.bar`的结果居然是2：


原因也显而易见：因为前面我们修改了foo的原型`foo.__proto__.bar = 2`，而foo是一个Object类的实例，所以实际上是修改了Object这个类，给这个类增加了一个属性bar，值为2。

后来，我们又用Object类创建了一个zoo对象`let zoo = {}`，zoo对象自然也有一个bar属性了。

那么，在一个应用中，如果攻击者控制并修改了一个对象的原型，那么将可以影响所有和这个对象来自同一个类、父祖类的对象。这种攻击方式就是**原型链污染**。



