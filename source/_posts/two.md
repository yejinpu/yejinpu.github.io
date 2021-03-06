---
title: ES6更新汇总（二）
date: 2017-12-12 17:38:58
tags: ES6
categories: javascript
---
# 一、ES6之迭代器(iterator)和for...of循环

## 1.1	什么是迭代器

1. 迭代器是一个对象
2. 迭代器提供一个方法next() 这个方法总是能够返回迭代到的对象。
3. next返回的对象中，至少有两个属性：done 是一个boolean值(表示数据是否迭代完)。  value：具体的数据(迭代到的具体数据)

 ​迭代器只是带有特殊接口(方法)的对象。所有迭代器对象都带有 next() 方法并返回一个包含两个属性的结果对象。这些属性分别是 value 和 done，前者代表下一个位置的值，后者在没有更多值可供迭代的时候为 true 。迭代器带有一个内部指针，来指向集合中某个值的位置。当 next() 方法调用后，指针下一位置的值会被返回。

 ​若你在末尾的值被返回之后继续调用 next()，那么返回的 done 属性值为 true，value 的值则由迭代器设定。该值并不属于数据集，而是专门为数据关联的附加信息，如若该信息并未指定则返回 undefined 。迭代器返回的值和函数返回值有些类似，因为两者都是返回给调用者信息的最终手段。

> 具有迭代器接口的结构有哪些：

```javascript
var str = new String("abcd");
var obj = {name : "小雪", age : 18};
var arr = ["aa", "bb", "cc"];
var set = new Set(arr);
var map = new Map([['name', "小黑"], ['age', 18]]);

/*** 字符串 ***/
console.log(str[Symbol.iterator]); //是一个函数

var iteratorStr = str[Symbol.iterator]();
console.log(iteratorStr.next());
console.log(iteratorStr.next());
console.log(iteratorStr.next());
console.log(iteratorStr.next());
console.log(iteratorStr.next());
console.log(iteratorStr.next());

var iteratorStr2 = str[Symbol.iterator]();
console.log(iteratorStr2.next());
console.log(iteratorStr2.next());
console.log(iteratorStr2.next());
console.log(iteratorStr.next());
console.log(iteratorStr2.next());
console.log(iteratorStr2.next());
console.log(iteratorStr2.next());

/*** 自定义对象不具备迭代器接口 ***/

/** 数组 **/
var iteratorArr = arr[Symbol.iterator]();
console.log(iteratorArr.next());
console.log(iteratorArr.next());
console.log(iteratorArr.next());
console.log(iteratorArr.next());

/** set **/
var iteratorSet = set[Symbol.iterator]();
console.log(iteratorSet.next());
console.log(iteratorSet.next());
console.log(iteratorSet.next());
console.log(iteratorSet.next());

/** map **/
var iteratorMap = map[Symbol.iterator]();
console.log(iteratorMap.next());
console.log(iteratorMap.next());
console.log(iteratorMap.next());
console.log(iteratorMap.next());
```

## 1.2 迭代器与for..of的关系

```javascript
 //重写一下字符串Symbol.iterator 接口
		String.prototype[Symbol.iterator] = function () {
			console.log("===========");
			//索引
			var idx = 0;
			var _this = this;
			return {
				next : function () {
					return {
						value : _this[idx++],
						done : idx - 1 < _this.length ? false : true
					}
				}
			}
		}
        
    //使用重写后的接口    
    var iteratorStr3 = str[Symbol.iterator]();
    console.log(iteratorStr3.next());
    console.log(iteratorStr3.next().value);
    console.log(iteratorStr3.next().value);
    console.log(iteratorStr3.next());
    console.log(iteratorStr3.next());

    var iteratorStr4 = str[Symbol.iterator]();
    console.log(iteratorStr4.next());
    console.log(iteratorStr4.next());
    console.log(iteratorStr4.next());
        
      //编写的同时使用for..of来遍历 字符串   
       for (var chr of str) {
			console.log(chr);
		}
```

> 经过以上观察，迭代器的实现直接影响for..of的使用。所以for..of循环的底层实现就是迭代器的实现。



# 二、ES6之对象功能的扩展

> 在JavaScript中，几乎所有的类型都是对象，所以使用好对象，对提升JavaScript的性能很重要。
>
> **ECMAScript 6 给对象的各个方面，从简单的语法扩展到操作与交互，都做了改进。**

## 1.1	对象类别

> ECMAScript 6 规范明确定义了每种对象类别。理解该术语对于从整体上认识该门语言显得十分重要。对象类别包括：

- 普通对象（ordinary object）拥有 JavaScript 对象所有的默认行为。
- 特异对象（exotic object）的某些内部行为和默认的有所差异。
- 标准对象（standard object）是 ECMAScript 6 中定义的对象，例如 Array, Date 等，它们既可能是普通也可能是特异对象。
- 内置对象（built-in object）指 JavaScript 执行环境开始运行时已存在的对象。标准对象均为内置对象。

 ## 1.2对象字面量的语法扩展

 ### 1.2.1简写的属性初始化

```javascript
<script type="text/javascript">
    function createPerson(name, age) {
        //返回一个对象：属性名和参数名相同。
        return {
            name:name,
            age:age
        }
    }
    console.log(createPerson("lisi", 30)); // {name:"lisi", age:30}
    //在ES6中，上面的写法可以简化成如下形式
    
</script>
```

> **在ES6中，上面的写法可以简化成如下的形式：**

```javascript
<script type="text/javascript">
    function createPerson(name, age) {
        //返回一个对象：属性名和参数名相同。
        return {
            name,  //当对象属性名和本地变量名相同时，可以省略冒号和值
            age
        }
    }
    console.log(createPerson("lisi", 30)); // {name:"lisi", age:30}
</script>
```

*当对象字面量中的属性只有属性名的时候，JavaScript 引擎会在该作用域内寻找是否有和属性同名的变量。在本例中，本地变量 name 的值被赋给了对象字面量中的 name 属性。*

*该项扩展使得对象字面量的初始化变得简明的同时也消除了命名错误。对象属性被同名变量赋值在 JavaScript 中是一种普遍的编程模式，所以这项扩展的添加非常受欢迎。*

### 1.2.2	简写的方法声明

```javascript
<script type="text/javascript">
    var person = {
        name:'lisi',
        sayHell:function () {
            console.log("我的名字是：" + this.name);
        }
    }
    person.sayHell()
</script>
```

> 在ES6中，上面的写法可以简化成如下的形式：

```Javascript
<script type="text/javascript">
    var person = {
        name:'李四',
        sayHell() {
            console.log("我的名字是：" + this.name);
        }
    }
    person.sayHell()
</script>
```

*省略了冒号和function看起来更简洁*

### 1.2.3	在字面量中动态计算属性名

> 在ES6之前，如果属性名是个变量或者需要动态计算，则只能通过  对象.[变量名]  的方式去访问。而且这种动态计算属性名的方式 **在字面量中** 是无法使用的。

```Javascript
<script type="text/javascript">
    var p = {
        name : '李四',
        age : 20
    }
    var attName = 'name';
    console.log(p[attName]) //这里 attName表示的是一个变量名。
</script>
```

> 而下面的方式使用时没有办法访问到attName这个变量的。

```JavaScript
<script type="text/javascript">
    var attName = 'name';
    var p = {
        attName : '李四',  // 这里的attName是属性名，相当于各级p定义了属性名叫 attName的属性。
        age : 20
    }
    console.log(p[attName])  // undefined
</script>
```

> 在ES6中，把属性名用[ ]括起来，则括号中就可以引用提前定义的变量。

```JavaScript
<script type="text/javascript">
    var attName = 'name';
    var p = {
        [attName] : '李四',  // 引用了变量attName。相当于添加了一个属性名为name的属性
        age : 20
    }
    console.log(p[attName])  // 李四
</script>
```

## 1.3	新增的方法

> ECMAScript 从第五版开始避免在 Object.prototype 上添加新的全局函数或方法，转而去考虑具体的对象类型如数组）应该有什么方法。当某些方法不适合这些具体类型时就将它们添加到全局 Object 上 。
>
> ECMAScript 6 在全局 Object 上添加了几个新的方法来轻松地完成一些特定任务。

### 1.3.1	Object.is()

> 在 JavaSciprt 中当你想比较两个值时，你极有可能使用比较操作符（==）或严格比较操作符（===）。许多开发者为了避免在比较的过程中发生强制类型转换，更倾向于后者。但即使是严格等于操作符，它也不是万能的。例如，它认为 +0 和 -0 是相等的，虽然它们在 JavaScript 引擎中表示的方式不同。同样 NaN === NaN 会返回 false，所以必须使用 isNaN() 函数才能判断 NaN 。

> ECMAScript 6 引入了 Object.is() 方法来补偿严格等于操作符怪异行为的过失。该函数接受两个参数并在它们相等的返回 true 。只有两者在类型和值都相同的情况下才会判为相等。如下所示：

```javascript
console.log(+0 == -0);              // true
console.log(+0 === -0);             // true
console.log(Object.is(+0, -0));     // false

console.log(NaN == NaN);            // false
console.log(NaN === NaN);           // false
console.log(Object.is(NaN, NaN));   // true

console.log(5 == 5);                // true
console.log(5 == "5");              // true
console.log(5 === 5);               // true
console.log(5 === "5");             // false
console.log(Object.is(5, 5));       // true
console.log(Object.is(5, "5"));     // false
```

*很多情况下 Object.is() 的表现和 === 是相同的。它们之间的区别是前者 **认为 +0 和 -0 不相等而 NaN 和 NaN 则是相同的**。不过弃用后者是完全没有必要的。何时选择 Object.is() 与 == 或 === 取决于代码的实际情况。*

### 1.3.2	Object.assign()

> 使用assign主要是为了简化对象的混入（mixin）。混入是指的在一个对象中引用另一个对象的属性或方法。
>
> assing可以把一个对象的属性和方法完整的转copy到另外一个对象中。

```javascript
<script type="text/javascript">
    var p = {
        name : "lisi",
        age : 20,
        friends : ['张三', '李四']
    }
    var p1 = {};
    Object.assign(p1, p); //则p1中就有了与p相同的属性和方法.  p1是接受者，p是提供者
    console.log(p1);
    //这种copy是浅copy，也就是说如果属性值是对象的话，只是copy的对象的地址值(引用）
    console.log(p1.friends == p.friends);  //true	p1和p的friends同事指向了同一个数组。
    p.friends.push("王五");
    console.log(p1.friends); //['张三', '李四', '王五']
</script>
```

> assign方法可以接受任意多的提供者。意味着后面提供者的同名属性和覆盖前面提供者的属性值。

```JavaScript
<script type="text/javascript">
    var p = {
        name : "lisi",
        age : 20,
        friends : ['张三', '李四']
    }
    var p1 = {
        name : 'zs',
    }
    var p2 = {};
    Object.assign(p2, p, p1); //p和p1都是提供者
    console.log(p2.name); // zs
</script>
```

# 三、ES6之全新的函数：箭头函数（=>)

> ECMAScript 6 最有意思的部分之一就是箭头函数。正如其名，箭头函数由 “箭头”（=>）这种新的语法来定义。
>
> 其实在别的语言中早就有了这种语法结构，不过他们叫拉姆达表达式。

## 4.1	箭头函数语法

> 基本语法如下：

```javascript
(形参列表)=>{
  //函数体
}
```

------

> 箭头函数可以赋值给变量，也可以像匿名函数一样直接作为参数传递。

- 示例1：

```javascript
<script type="text/javascript">
    var sum = (num1, num2) =>{
        return num1 + num2;
    }
    console.log(sum(3, 4));
    //前面的箭头函数等同于下面的传统函数
    var add = function (num1, num2) {
        return num1 + num2;
    }
    console.log(add(2, 4))
</script>
```

------

> 如果函数体内只有一行代码，则包裹函数体的 **大括号** ({ })完全可以省略。如果有return，return关键字也可以省略。
>
> 如果函数体内有多条语句，则 {} 不能省略。

- 示例2：

```javascript
<script type="text/javascript">
    var sum = (num1, num2) => num1 + num2;
    console.log(sum(5, 4));
    //前面的箭头函数等同于下面的传统函数
    var add = function (num1, num2) {
        return num1 + num2;
    }
    console.log(add(2, 4));

	//如果这一行代码是没有返回值的，则方法的返回自也是undefined
	var foo = (num1, num2) => console.log("aaa");
	console.log(foo(3,4));  //这个地方的返回值就是undefined
</script>
```

------

> 如果箭头函数只有一个参数，则包裹参数的小括号可以省略。其余情况下都不可以省略。**当然如果不传入参数也不可以省略**

- 示例3：

```javascript
<script type="text/javascript">
    var foo = a=> a+3; //因为只有一个参数，所以()可以省略
    console.log(foo(4)); // 7
</script>
```

------

> 如果想直接返回一个js对象，而且还不想添加传统的大括号和return，则必须给整个对象添加一个**小括号 ()**

- 示例4：

```Javascript
<script type="text/javascript">
    var foo = ()=>({name:"lisi", age:30});
    console.log(foo());
	//等同于下面的；
	var foo1 = ()=>{
      	return {
          	name:"lisi",
          	age : 30
      	};
	}
</script>
```

## 4.2	使用箭头函数实现函数自执行

```Javascript
<script type="text/javascript">
    var person = (name => {
            return {
                name: name,
                age: 30
            }
        }
    )("zs");
    console.log(person);
</script>
```

## 4.3	箭头函数中无this绑定(No this Binding)

> 在ES5之前this的绑定是个比较麻烦的问题，稍不注意就达不到自己想要的效果。因为this的绑定和定义位置无关，只和调用方式有关。
>
> **在箭头函数中则没有这样的问题，在箭头函数中，this和定义时的作用域相关，不用考虑调用方式**
>
> 箭头函数没有 this 绑定，意味着 this 只能通过查找作用域链来确定。**如果箭头函数被另一个不包含箭头函数的函数囊括，那么 this 的值和该函数中的 this 相等，否则 this 的值为 window。**

```Javascript
<script type="text/javascript">
    var PageHandler = {
        id: "123456",
        init: function () {
            document.addEventListener("click",
                event => this.doSomething(event.type), false); // 在此处this的和init函数内的this相同。
        },

        doSomething: function (type) {
            console.log("Handling " + type + " for " + this.id);
        }
    };
    PageHandler.init();
</script>
```

看下面的一段代码：

```javascript
<script type="text/javascript">

    var p = {
        foo:()=>console.log(this)   //此处this为window
    }
    p.foo();  //输出为 window对象。   并不是我想要的。所以在定义对象的方法的时候应该避免使用箭头函数。
//箭头函数一般用在传递参数，或者在函数内部声明函数的时候使用。
</script>
```

> 说明：

1. 箭头函数作为一个使用完就扔的函数，不能作为构造函数使用。也就是不能使用new 的方式来使用箭头函数。
2. 由于箭头函数中的this与函数的作用域相关，所以不能使用call、apply、bind来重新绑定this。但是虽然this不能重新绑定，但是还是可以使用call和apply方法去执行箭头函数的。

 ## 4.4无arguments绑定

> 虽然箭头函数没有自己的arguments对象，但是在箭头函数内部还是可以使用它外部函数的arguments对象的。

```javascript
<script type="text/javascript">
    function foo() {
        //这里的arguments是foo函数的arguments对象。箭头函数自己是没有 arguments 对象的。
        return ()=>arguments[0]; //箭头函数的返回值是foo函数的第一个参数
    }
    var arrow = foo(4, 5);
    console.log(arrow()); // 4
</script>
```
