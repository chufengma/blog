### JavaScript 原型与原型链

#### __proto__, prototype  

JavaScript依赖原型与原型链来实现继承。在JavaScript世界里面只有Function，Object，和基本类型，没有Java语言里面类似的Class。所以JavaScript的继承其实是对象的继承，不是类的继承。JavaScritp通过原型和原型组成的链来实现这一机制。

JavaScript中任意对象都有一个原型对象，对象从原型对象继承属性和方法。通常`[object].__proto__` 属性的值就对应他的原型对象。Function 比一般对象多了一个`prototype`属性，表示原型，他是一个带有构造函数的对象。

JavaScript中创建任意对象的时候，会给这个对象创建一个`__proto__`属性，这个属性就是他的原型对象。JS对象从原型对象上继承属性和方法。创建任意函数的时候，给这个函数创建`__proto__`属性的同时也创建一个`prototype`属性, `prototype` 就是函数的原型，它约定了函数的行为。`prototype` 属性是一个带有`constructor`属性的对象。

可以这么理解：
```
Object = { [__proto__] , ... }
Function = Function { [prototype] , [__proto__], ... } 
prototype = { constructor: [Function] }
```

结合实际例子：
```
// class 实际上是Function的语法糖。
class A {
	name : "123123"
}

class B extends A {
	age : 123123
}

let a = new A();
let b = new B();

A 有一个__proto__ 属性，指向A对象的构造函数。
A 有一个prototype属性，他是一个对象，{constructor : [A的构造函数]}

B 有一个__proto__属性，指向一个B的构造函数。
B 有一个prototype属性，他是一个对象，{constructor : [B的构造函数]}

a 有一个__proto__属性，指向一个Object对象。
b 有一个__proto__属性，指向一个A对象。
```

#### 原型链

JavaScript通过上面的原型和原型对象的配合来实现继承。

在A对象需要继承B对象的时候，先创建一个A构造函数，然后将A.__proto__ 指向B对象的构造函数，然后创建一个A.prototype对象，将A.prototype.constructor指向A的构造函数。
...待续
