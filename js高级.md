---

---

# 									闭包

## 1.变量的作用域

- 前提:这里只全部都通过var 创建的变量或对象

### 1.1.[全局变量]:函数外创建的变量



```js
   var x=10;
   function test(){
    alert("全局变量在test函数中"+x);
   function a() {
     alert("全局变量在私有函数a中"+x);
   }
   a();
  }
```



### 1.2.[局部变量]:函数内部创建的变量 

- 2.局部变量:函数内部创建的变量 

```js
  var x=10;
  function test(){
  var y=20;
   alert("全局变量在test函数中"+x);
   alert("局部变量在test函数中"+y);
   //alert("局部变量在test函数中"+z);//报错z未定义
   function a(){
     var z=30;
     alert("局部变量在函数a中"+y);
   }
   a();
 }
```

注:x是全局变量，整个作用域有效

  y是局部变量在test函数中有效，在a函数中有效

  z是局部变量只在a函数中有效

思考:如果我们想在函数外使用函数内的变量怎么办？

<img src="./img/闭包图片.png" alt="1577954520988" style="zoom: 33%;" />

## 2.闭包

1.闭包的概念:是指有权访问一个函数作用域中的变量的函数。创建闭包常见的方式，就是在一个函数内部创建另一个内部（私有）函数

### 2.1闭包实例：

#### 1.  闭包-1

 在函数外部直接调用局部变量x，报错

```js
   function test(){
   var x=10;
   return function(){
     return x;
   };
 }
 //调用局部变量x，报错未定义
 //alert(x);
 // [调用]
   var a=test();
   alert(a());
```

**说明:a实际上就是闭包匿名函数。函数test中的局部变量x一直保存在内存中，并没有在test调用后被自动清除。**

#### 2.  闭包-2

```js
  var y;
  function test(){
   var x=10;
   y= function(){
     return x;
   }
 }
//调用函数
   test();
//[调用]
   alert(y());
```

**说明:y实际上就是闭包y函数。函数test中的局部变量x，并没有在test函数调用后删除，x一直保留在内存中。y函数依赖于test函数都一直保存在内存中，不会被垃圾回收机制（garbage collection）回收。**

####  3.  闭包-3

```js
   function test(arg){
   var y=function(){
     return arg;
   }
   arg++;
   return y;
 }

 //[调用]
 var a=test(123);
 alert(a());
```

#### 4.  闭包-4

```js
   function test(){
       var x=[];
       var i;
       for(i=0;i<3;i++){
          x[i]=function(){
               return i;
           }
       }
       return x;
  }
        //调用
	var a=test();
	alert(a[0]());
	alert(a[1]());
	alert(a[2]());
```

说明:输出结果都是3，因为循环完毕i的值为3，这三个函数都指向这一个共同的值，如和解决这个问题?

#### 4.  闭包-4-1使用自调函数来实现  

```js
   function test(){
   var x=[];
   var i;
   for(i=0;i<3;i++){
      x[i]=(function(a){
          return function(){
              return a;
          }
      })(i);
   }
       return x;
   }
       //调用
	var a=test();
	alert(a[0]());
	alert(a[1]());
	alert(a[2]());

```

#### 4.  闭包-4-2不使用自调函数来实现

```js
   function test(){
       function makeClosure(x){
           return function(){
               return x;
           }
       }
       var a=[];
       var i;
       for(i=0;i<3;i++){
           a[i]=makeClosure(i);
       } 
       return a;
   }
      //调用
	var b=test();
	alert(b[0]());
	alert(b[1]());
	alert(b[2]());

```

### 2.2  闭包应用实例--Getter与Setter

1.实现了封装

```js
	var getValue,setValue;
	(function(){
        var secret=0;
        getValue=function(){
            return secret;
        };
        setValue=function(v){
            secret=v;
        };
    })();
        //调用
	alert(getValue());//查看内部局部变量值
	setValue(3);//设置内部局部变量的值
	alert(getValue());

```

### 2.3  迭代器实例

```js
	function setup(x){
        var i=0;
        return function(){
            return x[i++];
        };
    }
       //调用
	var next=setup(["a","b","c"]);
	alert(next());
	alert(next());
	alert(next());

```

### 闭包注意事项 ：

* 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，负责会造成网页的性能问题，在IE中可能导致内存泄漏。解决方法是，在退出函数之前，将不使用的局部变量全部删除。赋值为null;

* 闭包会在夫函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象(object)使用，把闭包当作它的公用方法(Public Method)，把内部变量当作他的私有属性(private value)，这时一定要小心，不要随便改变父函数内部变量的值。

 # 面向对象程序设计

## 1. Javascript 面向对象程序设计

概述：javascript不像其他面向对象编程语言那样有类的概念，JavaScript没有类（构造函数）的概念，只有对象的概念。

## 2 理解JavaScript中对象的概念

### 2.1理解JavaScript中对象的概念

+ 什么是对象

  ​	收音机、学生、手机

+ 什么是面向对象

  ​	使用对象的时候只关注对象提供的功能，而不关注其内部实现的细节

  ​	用对象

  ​	写对象

+ 面向对象编程是一种通用的思想，并非只有编程才用，任何事情都能使用。 

### 2.2 理解javascript中对象的概念

+ 面向对象编程（oop）特点

  ​	聚合：将所有的内容聚集到一起，生成一个集合

  ​	封装：不考虑内部实现，只考虑提供的功能

  ​	继承：从已有对象继承出新对象

  ​			多重继承

  ​	多态：可以以不同形式出现

+ 对象的组成

  ​	属性---变量（静态特性）

  ​	方法---函数（动态行为）


## 3.创建对象的方式

--原始模式

--工厂模式

--构造函数

--混合模式

--json方式

### 3.1 原始模式

+ 创建方法

  ​	var 对象名=new Object();

  ​	对象名.属性=“属性值”；

  ​	对象名.方法=function(){};

  调用方法：

  ​	对象名.属性；

  ​	对象名.方法；

### 3.2 工厂模式

+ 创建方法

  function 方法名 （属性值1，属性值2，...）{

  ​	var 对象名=new Object();

  ​	对象名.属性=“属性值”；

  ​	对象名.方法=function(){}

  ​	return 对象名；

  }

  调用方法：

  ​	var 对象名=方法名（实参1，实参2，...）；

  ​	对象.属性；

  ​	对象.方法（）

### 3.3 构造函数

+ 创建方法

  function 方法名（属性值1，属性值2，...）{

  ​	this.属性=“属性值”；

  ​	this.属性=“属性值”；

  ​	this.方法=function(){};

  }

  调用方法：

  ​	var 对象名=new 方法名(实参1，实参2，...);

### 3.4 混合模式

+ 创建方法

  function 方法名（形参，形参...）{

  ​		对象.prototype.属性=属性值；

  ​		对象.prototype.方法=function(){};

  }

  调用方法：

  ​	var 对象=new 方法名(实参1，实参2...);

  ​	对象名.属性；

  ​	对象名.方法

### 3.5 json对象

+ JSON（JavaScript Object Notation）是一种轻量级的数据交换格式。JavaScript对象的一个子集（对象和数组混合组成的数据结构）。

+ 属性名--字符串

+ 属性值--属性值可以包含字符串、数值、布尔值、函数、数组、对象、null等

+ JavaScript对象和JSON关系：

  --json简单来说就是JavaScript中的对象和数组，所以这两种结构就是对象和数组两种结构，通过这两种结构可以表示各种复杂的结构

  --对象：对象在js中表示为”{}“括起来的内容，数据结构为{key:value,key:value,...}的键值对的结构，在面向对象的语言中，key为对象的属性；value为对应的属性值，所以很容易理解，取值方法为对象.key获取属性值，这个属性之的类型可以是 数字、字符串、数组、对象几种。

  --数组：数组在js中是中括号“[]”括起来的内容，数据结构为 ["java","javascript","vb",...]，取值方式和所有语言中一样，使用索引获取，字段值的类型可以是 数字、字符串、数组、对象几种。

  --经过对象、数组2种结构就可以组合成复杂的Json数据结构了。

  JSON特点:

  ​     --JSON 是纯文本

  ​     --JSON 具有“自我描述性”（人类可读）

  ​     --JSON 具有层级结构（值中存在值）

  ​     --JSON 可通过 JavaScript 进行解析

  ​     --JSON 数据可使用 AJAX 进行传输

### 3.6 json对象--实例

```json
var users={
"userName":"李三",
"pwd":"123456",
"positions":"技术部经理",
"employees": [
        { "eName":"李明","phone":"133422334"},
        { "eName":"张萍","phone":"183222334"},
        { "eName":"郑鑫","phone":"153442223"}
 	]
}
document.write("输出Json内容:<br>");
        document.write("用户名是:"+users.userName+"<br>");
        document.write("密码是:"+users.pwd+"<br>");
        document.write("职位是:"+users.positions+"<br>");
        for(var i=0;i<users.employees.length;i++){
        document.write("雇员有:姓名是:"+users.employees[i].eName
		+"电话号码是:"+users.employees[i].phone+"<br>");
        }
```

### 3.7 json对象--json字符串转js对象(eval)

``` json
    var str='{"name":"wangsan","age":19,"sex":"男","sdept":"cs"}';
    var str_json=eval("("+str+")");
    document.write("name:"+str_json.name+"<br>");
    document.write("age:"+str_json.age+"<br>");
    document.write("sex:"+str_json.sex+"<br>");
    document.write("sdept:"+str_json.sdept+"<br>");
```

***也可使用js中的JSON.parse(str)***

***总结：******什么是面向对象：面向对象是一种思想，只注重功能，不注重过程***

  ***面向对象的特点：聚合、封装、继承、多态***

  ***创建对象的方法:***

​    ***1.原始模式***

​    ***2.工厂模式***

​    ***3.构造函数***

​    ***4.混合模式***

​    ***5.json模式***

  ***json格式的字符串转化成js对象的方法：eval()、JSON.parse()***

### 3.8 this的使用及指向

1. 函数创建时产生一个this

   ```js
   function box(){alert(this)};//指向window
   ```

2. (谁调用指向谁)当有事件绑定，并执行了事件处理程序时，谁绑定的事件，事件处理程序中的this指向谁。

   ```js
   btn.onclick = function(){alert(this)};//绑定了事件，this事件的绑定者。
   ```

3. 回调函数中的this指向window

   ```js
   setTimeout(function(){alert(this)});//任何地方使用定时器，里面的this都指向window
   ```

4. 匿名函数可以使用bind()改变this地指向call()和apply()可以改变函数中地this指向

   ```js
   setTimeout(function(){alert(this)}.bind(btn));//使用bind()方法可以改变匿名函数this指向。
   ```

5. 箭头函数本身没有this，它的this是继承父级而来。

   ```js
   btn.onclick = () =>{alert(this)};//箭头函数的this指向父级执行环境，这里为window
   ```

6. 箭头函数不能使用call()、apply()和bind()来改变this指向

   ```js
   setTimeout(()=>{alert(this)}.bind(this));//箭头函数无法用call()、apply()和bind()来改变this指向
   ```

7. 严格模式下，全局函数的this是undefined；

   "use strict";

   ```js
   function box(){alert(this)l//严格模式下，this为undefined；
   ```

8. 面向对象下的this

   使用new关键字创建一个实例对象，构造函数中的this指向了该实例对象，同时实例对象的this指向构造函数的原型对象。

   ```js
   function Box(name){this.name = name;}
   Box.prototype.fun=function(){this};
   new Box("tom").run();//this指向Box.prototype。
   ```

## 4. 对象的继承

### 4.1第一种继承（属性）

``` js
function classA(scolor){
    this.color=scolor;
    this.sayColor=function(){
        alert(this.color);
    }
}
function classB(scolor,sname){
    this.newMethod=classA;//创建一个属性，将classA赋值给newMethod
    this.newMethod(scolor);//调用classA
    delete this.newMethod;//防止下面的this指向newMethod，删除。
    this.name=sname;
    this.sayName=function(){
        alert(this.name);
    }
}
var objA = new classA("blue");//实例化classA
var objB = new classB("red","john");//实例化classB
objA.sayColor();
objB.sayColor();
objB.sayName();
```

### 4.2第二种继承（call bind array）

+ 属性继承call
+ --说明:call应用

```js
function A(){
	alert(this);
}
A.call(12);//改变函数调用时的对象
```

语法结构：call(对象,参数1，参数2，...);

属性展示apply

​	语法结构：

​	apply(对象，new array(参数1，参数2))；

``` js
function Animal(){
    this.name = "Animal";
    this.showName = function(){
        alert(this.name);
    }
}
function Cat(){
    this.name = "Cat";
}
var animal = new Animal();
var cat = new Cat();
Animal.showName.call(cat,"");
cat.showName();
//Animal.showName.apply(cat,[]); 
//Animal.bind(cat)();//后面的括号是调用一次，这也是和call的区别。
//call和apply用法都是一样的，区别就在参数上边，call的参数使用字符串继承的apply使用数组来继承的。
//bind和call调用一样，也用字符串。
//用法：构造函数.call/apply/(对象，参数)；
```

***1. this的指向***

***2. 继承：***

 	1. 使用属性继承
 	2. call、bind、apply
 	3. 原型链继承
 	4. 构造函数继承
 	5. 组合继承
 	6. 寄生组合继承

### 4.3 原型链继承

+ 在JavaScript中，每个函数都会初始化一个属性，原型（prototype），当我们需要访问这个函数的某个属性时，就会去到prototype中寻找这个属性，若没有找到这个属性，prototype中也存在自己的prototype，于是乎就这样一直网上查找。

+ 当访问一个对象的某个属性时，会先在这个对象本身属性上查找，如果没有找到，则会去他的_ proto _隐式原型上查找，即它的构造函数的prototype，如果还没有找到就会再在构造函数的prototype的 _ proto _中查找，这样一层一层向上查找就会形成一个链式结构，我们称为原型链。

补充：

prototype:是一个原型对象，可以存在任何的构造函数中，我们之前说到的所有内置对象，在创建的时候都是用的new进行实例化的，那换句话来说，这些内置对象也是一个一个的构造函数：    

```js
 String()
```

如果想要扩展String这个构造函数的属性和方法的话，那么我们就需要把这些属性和方法在string这个构造函数的原型对象上：

String.prototype.属性名/方法名=值

如果在值中调用this的情况下，那么这个this执行的是

```js
new String("tom").run();//这个this指向String.prototype
```

_ proto_:引用属性，隐形属性，当使用prototype找不到构造函数的情况下，会使用 _ proto _这个隐式属性进行查找，在查找的过程中会一级一级的往上找，直到找到的object时null为止，在找的过程中会形成一个链式操作，那么这个链式操作称之为原型链。

```js
function Person (name) {
    this.name = name;
};
Person.prototype.getName = function () {	//对原型进行扩展
    return this.name;
};
function Parent (age) {
    this.age = age;
};
Parent.prototype = new Person('老明');	
//这一句是关键//通过构造器函数创建出一个新对象，把老对象的东西都拿出来。
Parent.prototype.getAge = function () {
    return this.age;
};
//Parent.prototype.getName = function(){   //可以重写从父类继承来的方法，会优先调用自己的。
//	console.log(222)
//};
var result = new Parent(22);
console.log(result.getName());	//老明 //调用了从Person原型中继承来的方法（继承到了当前对象的原型中）
console.log(result.getAge());	//22	//调用了从Parent原型中扩展来的方法
```

 原型链：prototype和_ proto_,原型，如果prototype找不到的话使用 _proto _一层一层往上找，直到找到object为null为止，这个链式结构称为的是原型链。

### 4.4 构造函数继承

``` js
function Person (name) {	//父类
    this.name = name;
    this.friends = ['小李','小红'];
    this.getName = function () {
        return this.name;
    }
};
function Parent (age) {		//子类
    Person.call(this,'老明');	//这一句是核心关键
    //这样就会在新parent对象上执行person构造函数中定义的所有对象初始化代码，
    //结果parent的每个实例都会聚有自己的friends属性的副本
    this.age = age;
};
var result = new Parent(23);
console.log(result.name);	//老明
console.log(result.friends);	//['小李','小红']
console.log(result.getName());	//老明
console.log(result.age);	//23
console.log(result.getSex());	//这个会报错，调用不了父类原型上面扩展的方法

```

使用构造函数继承的特点：直接把父类中的属性和方法继承到子类的构造函数中，在继承过程中使用call()、bind()、array()

### 4.5 组合继承

``` js
function Person (name) {	//父类
    this.name =name;
    this.friends = ['小李','小红'];
};
Person.prototype.getName = function () {
    return this.name;
}
function Parent (age) {		//子类
    Person.call(this,'老明');		//这一步很关键
    this.age = age;
};
Parent.prototype = new Person('老明');	//这一步也很关键
var result = new Parent(24);
console.log(result.name);		
console.log(result.getName());	//老明
result.friends.push('小智');	
console.log(result.friends);	//['小李','小红','小智']
console.log(result.age);	//24
var result1 = new Parent(25);	
//通过借用函数都有自己的属性，通过原型享用公共得方法  console.log(result1.name);
console.log(result1.friends);	//['小李','小红']
```

call()、bind()、array()这个方法只能继承父类函数中的属性和方法，在父类原型对象上绑定的属性和方法是继承不了的

string-->如果想要扩展string这个构造函数中的属性和方法的情况下，那么这些属性和方法就必须要绑定在string构造函数的原型对象上，那么这些属性和方法，我们通过call是继承不了的，所以我们需要通过原型链来继承

prototype只能继承父类构造函数原型对象上边的属性和方法，不能继承父类构造函数中的属性和方法。

如果我们使用的是原型链来进行集成的，那么原型链上边this代表的就是父类.prototype

### 4.6 寄生组合继承

``` js
function Person (name) {	//父类
    this.name =name;
    this.friends = ['小李','小红'];
};
Person.prototype.getName = function () {
    return this.name;
}
function Parent (age) {		//子类
    Person.call(this,'老明');		//这一步很关键
    this.age = age;
};
(function (){
    var Super = function () {}	//创建一个没有实例方法的类	中间类
    Super.prototype = Person.prototype;	//直接把父类原型对象上边得属性和方法给中间类得原型对象上边了。
    Parent.prototype = new Supper();	//将实例作为子类的原型
})();
var result = new Parent(23);
console.log(result.name);
console.log(result.friends);
console.log(result.getNmae());
console.log(result.age);
```

组合继承和寄生组合继承的区别：

组合继承的话是直接使用call和prototype来继承到子类中的

寄生组合继承：通过call先把父类的构造函数终得属性和方法继承过来，然后在继承父类原型对象上边的属性和方法得时候，通过一个中间类进行继承

在这个过程中，person依然是父类，parent还是子类，使用中间类的好处就是，父类不用反复掉调用，只需要调用一次父类即可，如果不使用中间类的话，子类只要被实例化那么父类就会被调用，原型链不会变。

# 深拷贝和浅拷贝

+ 浅度拷贝：复制一层对象的属性，并不包括对象里面的为引用类型的数据，当改变拷贝的对象里面的引用类型时，源对象也会改变。
+ 深度拷贝：重新开辟一个内存空间，需要递归拷贝对象里的引用，直到子属性都为基本类型。两个对象对应两个不同的地址，修改一个对象的属性，不会改变另一个对象的属性。

## 浅拷贝

``` js
var a=[0,1,2,3,4];
b=a;
console.log(a===b);
//不管a也好还是b也好，那么他们两个指向的内存空间都是a所在的内存空间，所以a变都变
a[0]=1;
console.log(a,b);
```

## 深拷贝

``` js
function deepClone(obj){	
    //deepClone是实现对象拷贝的方法，参数obj是需要拷贝的源对象，使用完这个方法之后需要返回一个新的对象，也就是使用深拷贝，拷贝出来的对象，他会指向一个新的内存空间。
    let objClone = Array.isArray(obj)?[]:{};//是数组就创建的是数组，是对象就创建的是对象。
    if(obj && typeof obj === "object"){		//因为只有数组和对象才会存在拷贝，所以进行判断
    	for(key in obj){	//遍历数组
            if(obj.hasOwnProperty(key)){
                //判断obj子元素是否为对象，如果是，进行复制
                if(obj[key] && typeof obj[key] === "object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}
let a = [1,2,3,4]
b=deepClone(a);
a[0]=2;
console.log(a,b)
```

# 防抖和节流

+ 在进行窗口的resize、scroll,输入框内容校验等操作时，如果事件处理函数调用的频率无限制，会加重浏览器的负担，导致用户体验非常糟糕。此时我们可以采用debounce（防抖）和throttle（节流）的方式来减少调用频率，同时又不能影响实际效果。

## 防抖

+ 函数防抖（debounce）:当持续出发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定时间到来之前，又一次触发了事件，就重新开始延时。如下图，持续触发scroll事件时，并不执行handle函数，当1000毫秒内没有触发scroll事件时，才会延时触发scroll事件。

  ``` js
  function debounce (fn,wait) {
      var timeout = null;
      return function () {
          if(timeout !== null){
              clearTimeout(timeout);
          }
          timeout = setTimeout(fn,wait)
      }
  }
  //处理函数
  function handle () {
      console.log(Math.random());
  }
  //滚动事件5
  window.addEventListener('scroll',debounce(handle,1000));
  ```

  实现防抖的原理：利用的是延时器，在执行事件处理函数的时候不立即执行，让她有一个延迟事件，如果在这个延迟时间里不在调用的话，那么到延迟时间的话就执行，如果在延迟时间里，再次调用的话，就清空上一次的调用，在本次执行的基础上继续执行

## 节流

+ 函数节流（throttle）：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。节流通俗解释就比如我们水龙头防水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴。如下图，持续触发scroll事件时，并不立即执行handle函数，每隔1000毫秒才会执行一次handle函数。

+ 两种实现方法：

+ 时间戳和定时器。

+ 时间戳：

  ``` js
  var throttle = function (func,delay) {
      var prev = Date.now();			//第一次执行的时间
      return function () {			
          var context = this;			//window
          var args = arguments;		//实际参数，需要两个，(handle,1000)
          var now = Date.now();		//再一次触发的时间
          if(now - prev >= delay){	
              func.apply(context,args);
              prev = Date.now();		//触发的时间作为起始时间
          }
      }
  }
  function handle () {
      console.log(Math.random());
  }
  window.addEventListener('scroll',throttle(handle,1000));
  ```

+ 定时器：

  ``` js
  var throttle = function(func, delay) {            
      var timer = null;            
      return function() {                
          var context = this;               
          var args = arguments;                
          if (!timer) {                    
              timer = setTimeout(function() {                        
                  func.apply(context, args);                        
                  timer = null;                    
              }, delay);                
          }            
      }        
  }        
  function handle() {            
      console.log(Math.random());        
  }        
  window.addEventListener('scroll', throttle(handle, 1000));
  ```

  

