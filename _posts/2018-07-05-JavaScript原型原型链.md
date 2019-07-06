### JavaScript 原型与原型链

#### __proto__, prototype  

JavaScript依赖原型与原型链来实现继承。在JavaScript世界里面只有Function，Object，和基本类型，没有Java语言里面类似的Class。所以JavaScript的继承其实是对象的继承，不是类的继承。JavaScritp通过原型和原型组成的链来实现这一机制。

JavaScript中任意对象都有一个原型对象，对象从原型对象继承属性和方法。通常`[object].__proto__` 属性的值就对应他的原型对象。Function 比一般对象多了一个`prototype`属性，表示原型，他是一个带有构造函数的对象。

JavaScript中创建任意对象的时候，会给这个对象创建一个`__proto__`属性，这个属性就是他的原型对象。JS对象从原型对象上继承属性和方法。创建任意函数的时候，给这个函数创建`__proto__`属性的同时也创建一个`prototype`属性, `prototype` 就是函数的原型，它约定了函数的行为。`prototype` 属性是一个带有`constructor`属性的对象。


可以这么理解：
```
Object = { [__proto__] , ... }
Function = Function { [prototype] , [__proto__], ... } 
prototype = { constructor: [Function], [__proto__], ...  }
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

a 有一个__proto__属性，指向A.prototype。
b 有一个__proto__属性，指向B.prototype。
```

#### 原型链

JavaScript通过上面的原型和原型对象的配合来实现继承。

在创建A对象的时候，先创建一个A构造函数，然后将A.__proto__ 指向顶级对象Object的构造函数。然后创建一个A.prototype对象，将A.prototype.constructor指向A的构造函数, 将A.prototype.__proto__ 指向 Object.prototype。
当需要创建A实例的时候，会根据A.prototype.constructor创建一个实例，并将这个实例的__proto__对象指向B.prototype。

在B对象需要继承A对象的时候，先创建一个B构造函数，然后将B.__proto__ 指向A对象的构造函数，然后创建一个B.prototype对象，将B.prototype.constructor指向B的构造函数, 将B.prototype.__proto__ 指向A.prototype。
当创建一个B对象的时候，会根据B.prototype.constructor创建一个实例，并将这个实例的__proto__对象指向B.prototype。

对于JavaScritp顶级对象而言。`Function.__proto__ === Function.prototype`, `Function.prototype.__proto__ === Object.prototype`。`Object.prototype.__proto__ == null`, 原型链至此为顶。所有函数对象的原型都回溯到`Function.prototype`, 所有对象的原型最终都回溯到`Object.prototype`。

这样就形成了这样的链条：

```
Function.prototype.__proto__ == Object.prototype
A.__proto__ == Function.prototype
A.prototype == {__proto__ = Object.prototype, constructor=[Function A]}
B.__proto__ == [Function A]
B.prototype == {__proto__ = A.prototype, constructor=[Function B]}

a.__proto__ == A.prototype
a.__proto__.__proto__ = Object.prototype
b.__proto__ == B.prototype
b.__proto__.__proto__ == A.prototype
b.__proto__.__proto__.__proto__ == Object.prototype
```

#### 方便的理解
关于 `__proto__` 和 `prototype` 不管上面怎么解释都有点绕。这里总结一种方便的理解方法：`X.__proto__` 表示的X的原型，如果X是Object，那`X.__proto__` 肯定也是Object。X如果是Function，那么`X.__proto__` 肯定也是Function。`prototype` 永远只是Function的属性，`prototype.constructor` 永远指向这个Function本身，`prototype.__proto__` 永远指向上一级对象的prototype（这里的上一级要么是父对象，要么是“类”）。

当搜寻一个对象a的属性的时候，先从a本身获取，获取不到则查找a.__proto__,如果仍然获取不到，则查找a.__proto__.__proto__,直到....__proto__为null。

prototype的原型永远也是prototype，function的原型永远也是function。这才是原型真正的字面理解。
