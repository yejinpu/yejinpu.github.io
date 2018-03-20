---
title: ES6更新汇总（一）
date: 2017-12-15 18:44:46
tags: ES6
categories: javascript
---
# 一、ES6之let和const的使用

## 1.1块级作用域

> 在ES5之前，不存在块级作用域，在编程的时候很多时候会带来很多的不便，ES6新增了块级作用域，补足了这方面的缺陷。

块级声明指的是该声明的变量无法被代码块外部访问。块作用域，又被称为词法作用域（lexical scopes），可以在如下的条件下创建：

- 函数内部
- 在代码块（即 {  }）内部

块级作用域是很多类C语言的工作机制，ECMAScript 6 引入块级声明的目的是增强 JavaScript 的灵活性，同时又能与其它编程语言保持一致。

## 1.2	let声明

> 使用let声明变量的语法和使用var声明的语法是一样的。**但是let声明的变量的作用域会限制在当前的代码块中。这是let与var的最大区别**。

```javascript
<script type="text/javascript">
    /***  var 和 ES6 let对比 ****/
		{
			var a = 10;
			let b = 20;
		}

		console.log(a);
		// 因为let声明后具有块级作用域 出了代码块后就访问不到 说报错
		console.log(b);
</script>
```

```javascript
<script type="text/javascript">
    //在for循环中的使用
      for (var i = 0; i < 5; i++) {

      }

      console.log(i);

      for (let j = 0; j < 5; j++) {

      }
      //这里 j 也是存在代码块中, 这里会报错
      console.log(j);
</script>
```

```javascript
<script type="text/javascript">
      //用var 声明的时候 有变量提升的特性
      console.log(c);
      var c = 'c';

      //使用let 不会有变量提升
      console.log(d);
      let d = 'd';
</script>
```

```javascript
<script type="text/javascript">
    var e = "e";
    if (true) {
      e = "ee";
      //在代码块中，只要存在let命令，它声明的所有变量都会绑定在它身上。
      let e = "小王";
      console.log(e);
    }
</script>
```

> 	总之在代码块内，使用变量之前，一定要用let声明好。
> 	如果let在使用变量之后声明的话，我们称为："暂时性死区" （temporary dead zone，简称：TDZ）。

```javascript
<script type="text/javascript">
    if (true) {
      //TDZ 开始
      d = "d";
      console.log(d)
      //TDZ 结束
      let d = "dd";
      console.log(d);
    }
</script>
```

> 	let不允许声明重复的变量

```javascript
<script type="text/javascript">
    (function () {
      let a = "a";
      //let a = "b";
    })();

    (function () {
      let a = "a";
      //var a = "b";
    })();
    			
    (function (arg) {
      //let arg = "a";
    })();
    			
    (function (arg) {
      {
        //自己在独立的一个块级里，就不会和形参的arg有冲突了。
        let arg = "a";
      }
    })();
</script>
```

> 	关于全局变量，在var声明的情况下，全局变量就等同于全局对象的属性，用ES6的let声明全局变量则不会。

```javascript
<script type="text/javascript">
    var str = 33;
    console.log(window.str); //33

    let str2 = "bcd";
    console.log(window.str2); //undefined
    console.log(str2); //bcd
</script>
```

> 	 利用let解决一个比较经典的问题

```javascript
<input type="button" value="按钮1" id="button1" />
<input type="button" value="按钮2" id="button2" />
<input type="button" value="按钮3" id="button3" />
<script type="text/javascript">
  	for (var z = 1; z < 4; z++) {
      var button = document.getElementById('button' + z);
        button.addEventListener('click', function () {
          alert('button' + (z));  //每次打印结果 都是 button4
        });
    }

    //利用函数立即执行、形成闭包。每次保存z的值 这样就可以实现我们想要的结果
    for (var z = 1; z < 4; z++) {
      var button = document.getElementById('button' + z);
      //每次循环把i存储起来
      (function (z) {
        button.addEventListener('click', function () {
          alert('button' + (z));
        });
      })(z);
    }
	
	//更简单的解决办法，形成块级作用域，使用let
	for (let z = 1; z < 4; z++) {
      var button = document.getElementById('button' + z);
        button.addEventListener('click', function () {
          alert('button' + (z));  //每次打印结果 都是 button4
        });
    }
</script>
```

## 1.3 const声明

 > 在  ES6 使用const来声明的变量称之为常量。这意味着它们不能再次被赋值。由于这个原因，所有的 const 声明的变量都必须在声明处初始化。
 >
 > 	const声明的常量和let变量一样也是具有块级作用域的特性，一样不支持变量（常量）提升，一样存在"TDZ"。
 > 	const命令用来声明常量，常量一旦声明，就无法改变。
 > 	const命令用来声明常量，一定要初始化。

```javascript
<script type="text/javascript">
    const PI = "3.1415..";
    console.log(PI);
    PI = "能修改吗？"; // 报错，常量不能修改
    console.log(PI); 
</script>
```

> 注意：针对于引用类型，修改值就是修改变量指针的指向。指向不同的对象实例
```javascript
<script type="text/javascript">
    const OBJ = {};
	//OBJ = { age : 18 };  不可以，这样相当于修改了对象的指针指向
    OBJ.name = "小雪";  //可以，这个没有修改对象的指针指向，而是修改内部的属性值
    console.log(OBJ);
    			
    //我们可以使用Object.freeze()冻结这个对象，从而达到不能操作对象的属性
    Object.freeze(OBJ);
    OBJ.age = 18;
    OBJ.name = "小虹";
    console.log(OBJ);
</script>
```


# 二、ES6之类

> 和大多数面向对象的语言（object-oriented programming language）不同，JavaScript 在诞生之初并不支持使用类和传统的类继承并作为主要的定义方式来创建相似或关联的对象。
>
> 这很令开发者困惑，而且在早于 ECMAScript 1 到 ECMAScript 5 这段时期，很多库都创建了一些实用工具（utility）来让 JavaScript 从表层上支持类。
>
> 尽管一些 JavaScript 开发者强烈主张该语言不需要类，但由于大量的库都对类做了实现，ECMAScript 6 也顺势将其引入。

## 2.1	ES5之前的模拟的类

​	在 ECMAScript 5 或更早的版本中，JavaScript 没有类。和类这个概念及行为最接近的是创建一个构造函数并在构造函数的原型上添加方法，这种实现也被称为自定义的类型创建，例如：

```javascript
function PersonType(name) {
    this.name = name;
}

PersonType.prototype.sayName = function() {
    console.log(this.name);
};

let person = new PersonType("Nicholas");
person.sayName();   // 输出 "Nicholas"

console.log(person instanceof PersonType);  // true
console.log(person instanceof Object);      // true
```

> 说明：

前面的PersonType我们以前一直叫做构造函数，其实他就是一个类型，因为他确实表示了一种类型。

## 2.2	ES6中基本的类声明

> 在ES6直接借鉴其他语言，引入了类的概念。所以再实现上面那种模拟 的类就容易了很多。

```javascript
//class关键字必须是小写。   后面就是跟的类名
class PersonClass {
    // 等效于 PersonType 构造函数。
    constructor(name) {  //这个表示类的构造函数。constuctor也是关键字必须小写。
        this.name = name;  //创建属性。  也叫当前类型的自有属性。
    } 
    // 等效于 PersonType.prototype.sayName.   这里的sayName使用了我们前面的简写的方式。
    sayName() {
        console.log(this.name);
    }
}
let person = new PersonClass("Nicholas");
person.sayName();   // 输出 "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

> 说明：

1. 自有属性：属性只出现在实例而不是原型上，而且只能由构造函数和方法来创建。在本例中，name 就是自有属性。我建议  **尽可能的将所有自有属性创建在构造函数中**，这样当查找属性时可以做到一目了然。
2. 类声明只是上例中自定义类型的语法糖。PersonClass 声明实际上创建了一个行为和 constructor 方法相同的构造函数，这也是 typeof PersonClass 返回 "function" 的原因。sayName() 在本例中作为 PersonClass.prototype 的方法，和上个示例中 sayName() 和 PersonType.prototype 关系一致。这些相似度允许你混合使用自定义类型和类而不需要纠结使用方式。

***虽然类和以前的使用构造函数+原型的方式很像，但是还是有一些不太相同的地方，而且要牢记***

1. 类声明和函数定义不同，**类的声明是不会被提升的**。类声明的行为和 let 比较相似，所以当执行流作用到类声明之前类会存在于暂存性死区（temporal dead zone）内。
2. 类声明中的代码自动运行在严格模式下，同时没有任何办法可以手动切换到非严格模式。
3. 所有的方法都是不可枚举的（non-enumerable），这和自定义类型相比是个显著的差异，因为后者需要使用 Object.defineProperty() 才能定义不可枚举的方法。
4. 所有的方法都不能使用 new 来调用，因为它们没有内部方法 [[Construct]]。
5. 不使用 new 来调用类构造函数会抛出错误。也就是  **必须使用new 类()**  的方式使用
6. 试图在类的方法内部重写类名的行为会抛出错误。（因为在类的内部，类名是作为一个常量存在的）



## 2.3	匿名类表达式

> 函数有函数表达式，类也有类表达式。
>
> 类表达式的功能和前面的类的声明是一样的。

```javascript
let PersonClass = class {

    // 等效于 PersonType 构造函数
    constructor(name) {
        this.name = name;
    }

    // 等效于 PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

let person = new PersonClass("Nicholas");
person.sayName();   // 输出 "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

## 2.4	作为一等公民的类型

> 在JavaScript中，函数是作为一等公民存在的。(也叫一等函数)。
>
> 类也是一等公民。

1. 类可以作为参数传递

```javascript
function createObject(classDef) {
    return new classDef();
}

let obj = createObject(class {
	constructor() {
        
    }
    sayHi() {
        console.log("Hi!");
    }
});

obj.sayHi();        // "Hi!"
```

2. 立即调用类构造函数，创建单例

```javascript
let person = new class {

    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }

}("Nicholas");

person.sayName();       // "Nicholas"
```

## 2.5	动态计算类成员的命名

> 类的成员，也可以像我们前面的对象的属性一样可以动态计算.(  使用[ ] 来计算)

```javascript
let methodName = "sayName";
class PersonClass {
    constructor(name) {
        this.name = name;
    }

    [methodName]() {
        console.log(this.name);
    }
}
let me = new PersonClass("Nicholas");
me.sayName();           // "Nicholas"
```

## 2.6	静态成员

> 在ES5中，我们可以直接给构造函数添加属性或方法来模拟静态成员。

```javascript
function PersonType(name) {
    this.name = name;
}
// 静态方法。  直接添加到构造方法上。  (其实是把构造函数当做一个普通的对象来用。)
PersonType.create = function(name) {
    return new PersonType(name);
};
// 实例方法
PersonType.prototype.sayName = function() {
    console.log(this.name);
};
var person = PersonType.create("Nicholas");
```

> 在上面的create方法在其他语言中一般都是作为静态方法来使用的。

**注意：**

ECMAScript 6 的类通过在方法之前使用正式的 ***static*** 关键字简化了静态方法的创建。例如，下例中的类和上例相比是等效的：

```javascript
class PersonClass {

    // 等效于 PersonType 构造函数
    constructor(name) {
        this.name = name;
    }

    // 等效于 PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }

    // 等效于 PersonType.create。
    static create(name) {
        return new PersonClass(name);
    }
}

let person = PersonClass.create("Nicholas");
```

> 注意：静态成员通过实例对象不能访问，只能通过类名访问！！！
>
> 通过和ES5模拟静态方法的例子你应该知道为啥了吧

## 2.7	继承

> 在ES6之前要完成继承，需要写很多的代码。看下面的继承的例子：

```javascript
<script type="text/javascript">
    function Father(name) {
        this.name = name;
    }
    Father.prototype.sayName = function () {
        console.log(this.name);
    }

    function Son(name,age) {
        Father.call(this, name);
        this.age = age;
    }
    Son.prototype = new Father();
    Son.prototype.constructor = Son;
    Son.prototype.sayAge = function () {
        console.log(this.age);
    }

    var son1 = new Son("儿子", 20);
    son1.sayAge();  //20
    son1.sayName(); //儿子

</script>
```

### 2.7.1	继承的基本写法

> 如果在ES6通过类的方式完成继承就简单了很多。
>
> 需要用到一个新的关键字：extends

```JavaScript
<script type="text/javascript">
    class Father{
        constructor(name){
            this.name = name;
        }
        sayName(){
            console.log(this.name);
        }
    }
    class Son extends Father{  //extents后面跟表示要继承的类型
        constructor(name, age){
            super(name);  //相当于以前的：Father.call(this, name);
            this.age = age;
        }
        //子类独有的方法
        sayAge(){
            console.log(this.age);
        }
    }

    var son1 = new Son("李四", 30);
    son1.sayAge();
    son1.sayName();
	console.log(son1 instanceof Son);  // true
	console.log(son1 instanceof Father);  //true

</script>
```

> 这种继承方法，和我们前面提到的构造函数+原型的继承方式本质是一样的。但是写起来更简单，可读性也更好。

***关于super的使用，有几点需要注意：***

1. 你只能在派生类中使用 super()，否则（没有使用 extends 的类或函数中使用）一个错误会被抛出。
2. 你必须在构造函数的起始位置调用 super()，因为它会初始化 this。任何在 super() 之前访问 this 的行为都会造成错误。也即是说super()必须放在构造函数的首行。
3. 在类构造函数中，唯一能避免调用 super() 的办法是返回一个对象。



### 2.7.2	在子类中屏蔽父类的方法（重写）

> 如果在子类中声明与父类中的同名的方法，则会覆盖父类的方法。(这种情况在其他语言中称之为 方法的覆写、重写 )

```javascript
<script type="text/javascript">
    class Father{
        constructor(name){
            this.name = name;
        }
        sayName(){
            console.log(this.name);
        }
    }
    class Son extends Father{  //extents后面跟表示要继承的类型
        constructor(name, age){
            super(name);  //相当于以前的：Father.call(this, name);
            this.age = age;
        }
        //子类独有的方法
        sayAge(){
            console.log(this.age);
        }
        //子类中的方法会屏蔽到父类中的同名方法。
        sayName(){
          	super.syaName();  //调用被覆盖的父类中的方法。 
            console.log("我是子类的方法，我屏蔽了父类：" + name);
        }
    }

    var son1 = new Son("李四", 30);
    son1.sayAge();
    son1.sayName();
</script>
```

> 如果在子类中又确实需要调用父类中被覆盖的方法，可以通过super.方法()来完成。
>
> 注意：
>
> 1. 如果是调用构造方法，则super不要加点，而且必须是在子类构造方法的第一行调用父类的构造方法
> 2. 普通方法调用需要使用super.父类的方法()  来调用。

### 2.7.3	静态方法也可以继承

```javascript
<script type="text/javascript">
   class Father{
       static foo(){
           console.log("我是父类的静态方法");
       }
   }
   class Son extends Father{

   }
   Son.foo(); //子类也继承了父类的静态方法。  这种方式调用和直接通过父类名调用时一样的。

</script>
```
# 三、ES6之set数据结构

​	JavaScript 在绝大部分历史时期内只有一种集合类型，那就是数组。数组在 JavaScript 中的使用方式和其它语言很相似，因为数组的索引只能是数字类型，当开发者觉得非数字类型的索引是必要的时候会使用非数组对象。这项用法促进了以非类数组对象为基础的 set 和 map 集合类型的实现。

> Set是类似数组的一种结构，可以存储数据，与数组的区别主要是  **Set中的元素不能重复，而数组中的元素可以重复**。
>
> 一句话总结：***Set类型是一个包含无重复元素的有序列表***

## 3.1	创建Set并添加元素

> Set本身是一个构造函数。

```javascript
<script type="text/javascript">
    //创建Set数据结构对象。
    var s = new Set();
    //调用set对象的add方法，向set中添加元素
    s.add("a");
    s.add("c");
    s.add("b");
	//set的size属性可以获取set中元素的个数
    console.log(s.size)
</script>
```

## 3.2	Set中不能添加重复元素

```javascript
<script type="text/javascript">
    var s = new Set();
    s.add("a");
    s.add("c");
    s.add("b");
    s.add("a");  //重复，所以添加失败。注意这个地方并不会保存。
    console.log(s.size); // 长度是3
</script>	
```

```javascript
<script type="text/javascript">
    var s = new Set();
    s.add(+0);
    s.add(-0);  //重复添加不进去
    s.add(NaN);
    s.add(NaN); //重复添加不进去
    s.add([]);
    s.add([]);  //两个空数组不相等，所以可以添加进去
    s.add({});
    s.add({});  // 两个空对象也不重复，所以也可以添加进去
    console.log(s.size); // 长度是6
</script>
```

## 3.3	使用数组初始化Set（解决数组去重问题）

```javascript
<script type="text/javascript">
    //使用数组中的元素来初始化Set，当然碰到重复的也不会添加进去。
    var s = new Set([2, 3, 2, 2, 4]);
    console.log(s.size)
</script>
```

## 3.4	判断一个值是否在Set中

> 使用Set的  has()  方法可以判断一个值是否在这个set中。

```javascript
<script type="text/javascript">
    let set = new Set();
    set.add(5);
    set.add("5");

    console.log(set.has(5));    // true
    console.log(set.has(6));    // false
</script>
```

## 3.5	移除Set中的元素

> delete(要删除的值)   ：删除单个值
>
> clear()：清空所有的值

```javascript
<script type="text/javascript">
    let set = new Set();
    set.add(5);
    set.add("5");

    console.log(set.has(5));    // true

    set.delete(5);

    console.log(set.has(5));    // false
    console.log(set.size);      // 1

    set.clear();

    console.log(set.has("5"));  // false
    console.log(set.size);      // 0
</script>
```

## 3.6	遍历Set

> 数组有个方法forEach可以遍历数组。
>
> 1. Set也有forEach可以遍历Set。
>
> 使用Set的forEach遍历时的回调函数有三个参数：
>
> function (value, key, ownerSet){
>
> }
>
> 参数1：遍历到的元素的值
>
> 参数2：对set集合来说，参数2的值和参数1的值是完全一样的。
>
> 参数3：这个 ==set== 自己

```javascript
<script type="text/javascript">
    let set = new Set(["a", "c", "b", 9]);
    set.forEach(function (v, k, s) {
        console.log(v + "   " + (v === k) + "  " + (s === set));   // 永远是true
    })

</script>
```

> for…of也可以遍历set。

```javascript
for(var v of set){
    console.log(v)
}

//所有的keys 和 values （获取的都是元素）
  for (var a of set.keys()) {
    console.log(a);
  }

  for (var a of set.values()) {
    console.log(a);
  }
```

------

***Set提供了处理一系列值的方式，不过如果想给这些值添加一些附加数据则显得力不从心，所以又提供了一种新的数据结构：Map

# 四、ES6之Map数据结构

​	ECMAScript 6 中的 map 类型包含一组有序的键值对，其中键和值可以是任何类型。

​	Map是一种数据结构，可以表示一种映射(键-值、key-value)关系的集合。

​	Map和JS中的对象字面量的功能，基本一样。

​	Map这种数据结构在其他语言里也都广泛使用。
​			例如：java中的HashMap,
​				  objective-c中的字典。
​			以上都是Map数据结构。

​	Map的特点是查找快，键不能重复。

## 4.1 创建Map对象和Map的基本的存取操作

> 1. Map创建也是使用Map构造函数
> 2. 向Map存储键值对使用set(key, value);方法
> 3. 可以使用get(key),来获取指定key对应的value

```javascript
<script type="text/javascript">
    var map = new Map();
    map.set("a", "lisi");
    map.set("b", "zhangsan");
    map.set("b", "zhangsan222");  // 第二次添加，新的value会替换掉旧的
    console.log(map.get("a"));
    console.log(map.get("b"));   //zhangsan222
	console.log(map.get("c")); //undefined.如果key不存在，则返回undefined
	console.log(map.size); //2
</script>
```

## 4.2	Map与Set类似的3个方法

- has(key) - 判断给定的 key 是否在 map 中存在
- delete(key) - 移除 map 中的 key 及对应的值
- clear() - 移除 map 中所有的键值对

 ## 4.3初始化Map

> 创建Map的时候也可以像Set一样传入数组。但是传入的数组中必须有两个元素，这个两个元素分别是一个数组。
>
> 也就是传入的实际是一个二维数组！

```javascript
<script type="text/javascript">
  //map接受一个二维数组
    var map = new Map([
      //每一个数组中，第一个是是map的可以，第二个是map的value。如果只有第一个，则值是undefined
        ["name", "lisi"],  
        ["age", 20],
        ["sex", "nan"]
    ]);
    console.log(map.size);
    console.log(map.get("name"))
</script>	
```

## 4.4	遍历Map

 forEach遍历

```javascript
<script type="text/javascript">
    var map = new Map([
        ["name", "李四"],
        ["age", 20],
        ["sex", "nan"]
    ]);
    /*
        回调函数有函数：
        参数1：键值对的value
        参数2：键值对的key
        参数3：map对象本身
     */
    map.forEach(function (value, key, ownMap) {
        console.log(`key=${key} ,vlue=${value}`);
        console.log(this);
    })
 </script>
```

for..of遍历

```javascript
// of 前面的参数是数组（每组键值对）
  for (var a of map) {
    console.log(a[0] + ":" + a[1]);
  }

  for (var [key, value] of map) {
    console.log(key + "-" + value);
  }

  //要所有的key
  for (var key of map.keys()) {
    console.log(key);
  }

  //要所有的value
  for (var value of map.values()) {
    console.log(value);
  }
```

# 五、额外扩展ES5的Object.defineProperty()

> Object.defineProperty() 是处理对象属性的一些扩展
>
> 三个参数 （都是必填项）
>
> 第一个参数：要设置属性所在的对象
>
> 第二个参数：要设置的属性名字
>
> 第三个参数：设置属性的特性

```javascript
var obj = {};
obj.a = "我是A";

Object.defineProperty(obj, 'userName', {
    // 设置初始值
    //注意：如果只设置这个特性的话，默认是不可修改的
    //换句话说，writable 属性的值是 false
    // value : '小雪',
    // writable : true,
    //是否可使用for..in遍历
    //默认是false 不可这个属性不遍历
    enumerable : true,
    //设置属性 存取器 (这个方法不能和 value、writable共存)
    //当使用obj.userName = "xxx" 的时候就会进入到这个方法内
    set : function (newSize) {
      console.log('set....');
      //这个value的功能 就是和 上面那个value一样，给size赋值用的
      if (typeof(newSize) == 'number') {
        value = newSize;	
      } else {
        console.log('类型错误，不能赋值！！！');
      }
    },
    //当使用obj.userName 获取的时候会调用这个方法
    get : function () {
      console.log('get....');
      //value同上。。。
      return value;
    }
  });
```

