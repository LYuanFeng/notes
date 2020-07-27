# JS 适记



### try/catch/finally 语句

**try/catch/finally** 语句用于处理代码中可能出现的错误信息。

**try**语句允许我们定义在执行时进行错误测试的代码块。（里面代码也是会执行的）

**catch** 语句允许我们定义当 **try** 代码块发生错误时，所执行的代码块。

**finally** 语句在 try 和 catch 之后无论有无异常都会执行

**注意：** catch 和 finally 语句都是可选的，但你在使用 try 语句时必须至少使用一个。

**提示：** 当错误发生时， JavaScript 会停止执行，并生成一个错误信息。使用 [throw](http://www.runoob.com/jsref/jsref-throw.html) 语句 来创建自定义消息(抛出异常)。如果你将 **throw** 和 **try** 、 **catch**一起使用，就可以控制程序输出的错误信息。

```
语法： try {
       有可能会出现异常的代码
    }catch(e){
       捕获到异常了之后要执行的处理-代码
       e就是异常信息
    }finally {
       不管有没有发生异常，都要执行的代码
       释放资源的处理
    }
```

```
/自己定义抛出异常。  throw
function getRes(num){
 if(isNaN(num)){
   //throw "你为什么不传一个数字捏？";
   throw {
     name:"计算错误异常",
     aaa:"你为什么不传一个数字捏？"
   };
 }
 var res = num *　2;
 return res;
}


try {
 var res1 = getRes("abc")
}catch(e) {
 console.log(e);
}
```



### promise 

**1、promise是什么？**

```
(1)主要用于异步计算
(2)可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果
(3)可以在对象之间传递和操作promise，帮助我们处理队列
(4)Promise的出现让我们告别回调函数，写出更优雅的异步代码
```

**2、为什么会有promise？**

```
为了避免界面冻结（任务）
同步：
假设你去了一家饭店，找个位置，叫来服务员，这个时候服务员对你说，对不起我是“同步”服务员，我要服务完这张桌子才能招呼你。那桌客人明明已经吃上了，你只是想要个菜单，这么小的动作，服务员却要你等到别人的一个大动作完成之后，才能再来招呼你，这个便是同步的问题：也就是“顺序交付的工作1234，必须按照1234的顺序完成”。
异步：
则是将耗时很长的A交付的工作交给系统之后，就去继续做B交付的工作，。等到系统完成了前面的工作之后，再通过回调或者事件，继续做A剩下的工作。
AB工作的完成顺序，和交付他们的时间顺序无关，所以叫“异步”。
```

**3、异步操作的常见语法**

```
（1）事件监听
（2）回调
```

**4、有了nodeJS之后...对异步的依赖进一步加剧了**

```
大家都知道在nodeJS出来之前PHP、Java、python等后台语言已经很成熟了，nodejs要想能够有自己的一片天，那就得拿出点自己的绝活：
无阻塞高并发，是nodeJS的招牌，要达到无阻塞高并发异步是其基本保障
举例：查询数据从数据库，PHP第一个任务查询数据，后面有了新任务，那么后面任务会被挂起排队；而nodeJS是第一个任务挂起交给数据库去跑，然后去接待第二个任务交给对应的系统组件去处理挂起，接着去接待第三个任务...那这样子的处理必然要依赖于异步操作
```

**5、异步回调的问题：**

```
、之前处理异步是通过纯粹的回调函数的形式进行处理
、很容易进入到回调地狱中，剥夺了函数return的能力
、问题可以解决，但是难以读懂，维护困难
、稍有不慎就会踏入回调地狱 - 嵌套层次深，不好维护
、一般情况我们一次性调用API就可以完成请求。
、有些情况需要多次调用服务器API，就会形成一个链式调用，比如为了完成一个功能，我们需要调用API1、API2、API3，依次按照顺序进行调用，这个时候就会出现回调地狱的问题
```

**6、promise的引入**

```
、promise是一个对象，对象和函数的区别就是对象可以保存状态，函数不可以（闭包除外）
、并未剥夺函数return的能力，因此无需层层传递callback，进行回调获取数据
、代码风格，容易理解，便于维护
、多个异步等待合并便于解决
```

**7、promise详解**

```
new Promise(
  function (resolve, reject) {
  	setTimeout(function() {
        // 一段耗时的异步操作
        resolve('成功') // 数据处理完成
        // reject('失败') // 数据处理出错
    }, 10000);
  }
).then(
  (res) => {console.log(res)},  // 成功
  (err) => {console.log(err)} // 失败
)
console.log('因为不是异步的，这句会执行比Promise快') //执行顺序1
```

上面代码console执行顺序：

以为Promise是异步，不影响外面代码执行

```
因为不是异步的，这句会执行比Promise快
成功
```

代码解读

```
resolve作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
reject作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
```

```
promise有三个状态：
1、pending[待定]初始状态
2、fulfilled[实现]操作成功
3、rejected[被否决]操作失败
当promise状态发生改变，就会触发then()里的响应函数处理后续步骤；
promise状态一经改变，不会再变。
```

```
Promise对象的状态改变，只有两种可能：
从pending变为fulfilled
从pending变为rejected。
这两种情况只要发生，状态就凝固了，不会再变了。
```



### Async/Await

**1、Async/Await简介**

```
async/await是写异步代码的新方式，优于回调函数和Promise。

async/await是基于Promise实现的，它不能用于普通的回调函数。===》（函数前面多了一个 async 关键字。await 关键字只能用在 async 定义的函数内。async 函数会隐式地返回一个 promise，该 promise 的 reosolve 值就是函数 return 的值。）

async/await与Promise一样，是非阻塞的。

async/await使得异步代码看起来像同步代码，再也没有回调函数。但是改变不了JS单线程、异步的本质。
```

**2、Async/Await的用法**

使用await，函数必须用async标识

await后面跟的是一个Promise实例

```
function getTimeout(time) {
        return new Promise(function(yes, no) {
           setTimeout(() => {
              yes('传哥');
           }, time);
        });
    }

    // 上面使用promise解决了异步回调嵌套不好维护的问题,
    // 但是我们还有更好方式书写上面的代码
    // async函数相当于使用同步的方式书写异步代码
    async function fn() {
        // await会等待promise成功,成功后在会向下走,直到遇到下一个await
        let a = await getTimeout(5000);
        console.log(`第一个定时器执行完毕了__${a}`);
        await getTimeout(3000);
        console.log('第二个定时器执行完毕了');
        console.log('加多少代码都可以');
        console.log('加多少代码都可以');
        await getTimeout(1000);
        console.log('第二个定时器执行完毕了');
    }

    fn();
```

当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。

**3、为什么 Async/Await 更好？**

```
1. 简洁
由示例可知，使用 Async/Await 明显节约了不少代码。我们不需要写.then，不需要写匿名函数处理 Promise 的 resolve 值，也不需要定义多余的 data 变量，还避免了嵌套代码。这些小的优点会迅速累计起来，这在之后的代码示例中会更加明显。

2. 错误处理
Async/Await 让 try/catch 可以同时处理同步和异步错误。在下面的 promise 示例中，try/catch 不能处理 JSON.parse 的错误，因为它在 Promise 中。我们需要使用.catch，这样错误处理代码非常冗余。并且，在我们的实际生产代码会更加复杂。
使用 async/await 的话，catch 能处理 JSON.parse 错误:
const makeRequest = async () => {
    try {
        // this parse may fail
        const data = JSON.parse(await getJSON());
        console.log(data);
    } catch (err) {
        console.log(err);
    }
};
```



### 数组与字符串的转换

##### 数组转字符串

```
（1） var arr = ['a', 'b', 'c']
​			arr.join('-'); // a-b-c
​			arr.join(''); //abc
```

##### 字符串转数组

```
（1） var str = 'ab+cd+e'
​			str.split('+') // ['ab', 'cd', 'e']
​			str.split('') // ['a', 'b', '+', 'c', 'd', '+', 'e']
```



### 判断语句中转换为false

```
在js中if条件为null/undefined/0/NaN/""表达式时，统统被解释为false,此外均为true哦
```



### String的常用方法

```
1》ES6提供三种检索方法（注意不兼容IE）

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。
  let s = 'Hello world!';
  s.startsWith('Hello') *// true*
  s.endsWith('!') *// true*
  s.includes('o') *// true

2》indexOf() 检索 返回第一次出现的位置，没有则返回 -1
var str="Hello world!"
str.indexOf("Hello") // 0
str.indexOf("WorldPP") // -1
str.indexOf("world") // 6

3》lastIndexOf()方法返回从右向左出现某个字符或字符串的首个字符索引值（与indexOf相反）
  var src="images/off_1.png";
  src.lastIndexOf('/'); //6
  src.lastIndexOf('g'); //15
  src.lastIndexOf('aaa'); //-1

4》substring(start,end)表示从start到end之间的字符串，包括start位置的字符但是不包括end位置的字符。
var src="images/off_1.png";
src.substring(7,10)//off

5》substr(start,length)表示从start位置开始，截取length长度的字符串。
var src="images/off_1.png";
src.substr(7,3)//off
```




### Array的常用方法

```
1》filter() 过滤筛选。filter把传入的函数依次作用于每个元素，然后根据返回值是 true 还是false决定保留还是丢弃该元素。三个参数 (当前元素, 索引,  当前数组)
​	var arr = [1, 5, 6]
​	var r = arr.filter((item,index, self) => {return item >= 5 ?  false :  true;}) 
	// r = [5, 6]
	
2》map() 遍历。不改变源数组，返回一个新的被改变过值之后的数组（map需return）。三个参数 (当前元素, 索引,  当前数组)实际上对数组的每个元素都遍历一次，同时返回一个新的值。记住一点是返回的这个数据的长度和原始数组长度是一致的。
​	var arr1 = [1, 2, 3]
​	var arr2 = arr1.map((item,index, self) => {return  item * item}) 
	// arr2 = [1, 4, 9]   ；  arr1 =  [1, 2, 3]
	
3》some()方法会依次执行数组的每个元素：
如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测。
如果没有满足条件的元素，则返回false。
	var arr1 = [1, 2, 3]
​	arr1.some((item,index, self) => item = == 2) //true

4》every()方法用于检测数组所有元素是否都符合指定条件（通过函数提供）。
如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
如果所有元素都满足条件，则返回 true。
var arr = [1, 2, 3]
​	arr.every((item,index, self) => item > 0) //true
	arr.every((item,index, self) => item > 2) //false
	
5》unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。
var arr= ['AA','BB']
arr.unshift('MM')//arr= ['MM','AA','BB']
arr.unshift('KK','HH')//arr= ['KK','HH','MM','AA','BB']

6》push() 方法可向数组的末尾添加一个或多个元素，并返回新的长度
var arr= ['AA','BB']
arr.push('MM')//arr= ['AA','BB','MM']

7》find() 方法，用于找出第一个符合条件的数组成员值为 true 的成员，然后返回该成员。如果没有符合条件的成员，则返回 undefined。(注意返回 undefined情況)
[1, 4, -5, 10].find((value, index, arr) => value < 0)// -5
[1, 4, -5, 10].find((n) => n < -6)// undefined

8》findIndex () 方法，find 方法非常类似，返回第一个符合条件的数组成员的 位置 ，如果所有成员都不符合条件，则返回-1。 两个方法都可以发现 NaN，弥补了数组的 IndexOf 方法的不足
[1, 5, 10, 15].findIndex((value, index, arr) => value > 9)//2
[1, 5, 10, 15].findIndex((value, index, arr) => value > 16)//-1
```



### 遍历

```
1》for循环，需要知道数组的长度，才能遍历（使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显）
​	for (j = 0,len=arr.length; j < len; j++) {
​	}

2》遍历对象， for （属性） in （对象）
​	let obj ={a:'2', b:3, c:true};
​		for (var i in obj) {
​		console.log(obj[i],i)  // 2, a
​	}

for--in 也可遍历数组 （i）为下标, 缺陷1. index索引为字符串型数字，不能直接进行几何运算。2. 遍历顺序有可能不是按照实际数组的内部顺序。 3.使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性
所以for in更适合遍历对象，不要使用for in遍历数组。(for -- of --可以解决这样的缺陷)
​	var arr = [7, 8, 9]
​	for (var a in arr) {
​		console.log(arr[a])  // 7, 8, 9
​	}

3》for --of --  可以与break、continue和return配合使用。（for 当前元素 of 需要遍历的数组）
​	var arr = [‘b’, 8, 9]
​	for (var a of arr) {
​		console.log(a)  // b, 8, 9
​	}

小总结: 
1、map()速度比foreach()快 
2、map():返回一个新的Array，不对原数组产生影响,每个元素为调用func的结果。 
3、foreach()不会产生新数组，没有返回值，只是针对每个元素调用func ,不能使用break语句中断循环，也不能使用	    return语句返回到外层函数
3、map()因为返回数组所以可以链式操作，foreach不能 
4、map()要比foreach()执行速度更快 
5、filter()返回一个符合func条件的元素数组 
6、some()返回一个boolean，判断是否有元素是否符合func条件 
7、every()返回一个boolean，判断每个元素是否符合func条件
```



### 构造函数

```
1.构造函数介绍
    a.构造函数命名使用一个名字，首字母大写。
    b.用new关键字配合使用。
    c.构造函数首先也是一个函数，只不过这个函数用来创建对象并且初始化对象。
    d.自动返回值。
  
2.构造函数执行过程。
    a.创建了一个对象
    b.让this指向new创建出来的对象。
    c.给这个对象的属性赋值、初始化
    d.自动返回那个创建出来的对象。

function Person(name,age){
    this.name = name;
    this.age = age;
    console.log(this);
}
var p1 = new Person("某敏",18);
console.log(p1); //{name: "某敏", age: 18}

3.注意
    a.构造函数不需要返回值，会自动返回创建的那个对象，但是如果真的给构造函数设置了返回值，会怎么样？
    如果是返回的简单的数据类型，会忽略；  如果返回的是复杂的数据类型，会把创建出来的那个对象给覆盖掉。

    b.构造函数首先也是一个函数，既然是函数，就可以不使用new关键配合着调用，而是直接调用。
    如果把构造函数当成普通的函数来调用，那这个构造函数中的this就是window。
    如果把构造函数当成普通的函数来调用，那他的返回值就是默认的返回值undefined。
    var pp = Person("琳琳",20);//没有使用new 关键字
    console.log(pp); //undefined
    console.log(window);
```



### 引入原型

```
1.原因：假如构造函数里有个sayHi方法，而这个方法是一样的，代码冗余。
    function Person(name,age){
        this.name = name;
        this.age = age;
        this.sayHi = function () {
        	console.log("你好，我的名字是"+this.name);
        }
    }
    var p1 = new Person("某敏",18);
    var p2 = new Person("某婷",19);
    p1.sayHi();
    p2.sayHi();
    console.log(p1.sayHi === p2.sayHi); //false 验证了sayhi p1/p2不是同一个  所以代码冗余

2.如何将sayHi方法变成公共的？ 存构造函数的原型- prototype里。
  原型其实就是一个对象，创建任意的构造函数的时候，系统都会帮我们自动的创建一个和这个构造函数关联的对象，这个对   象就是原型。
  function Person(name,age){
    this.name = name;
    this.age = age;
  }
  Person.prototype.sayHi = function () {
    console.log("你好，我的名字是"+this.name);
  }
  Person.prototype.type = "人类";
  //2.通过构造函数创建出来的对象，都可以访问这个构造函数对应的原型中的成员。
  var p1 = new Person("某敏",18);
  var p2 = new Person("某林林",38);
  console.log(p1.type +":"+ p2.type); //人类:人类
  console.log(p1.sayHi === p2.sayHi); //true
  console.log(p1.sayHi === Person.prototype.sayHi); //true
  
3.使用原型要注意的地方
	a.对象访问成员的规则： 是先看自己有没有这个成员，如果没有才去看原型中有没有。
        function Dog(type,name){
          this.type = type;
          this.name = name;
          this.bark = function () {
           console.log("嘿嘿嘿");
          }
        }
        //不管是那个狗，都可以叫，所以这种公共的方法，就可以写在原型中。
        Dog.prototype.bark = function () {
          console.log("汪汪汪...我的名字是"+this.name);
        }
        var d1 = new Dog("二哈","小2");
        d1.bark(); // 嘿嘿嘿
    
    b.原型中添加成员，就必须使用  构造函数.prototype  这种方式。
        function Dog(pinZhong,name){
          this.pinZhong = pinZhong;
          this.name = name;
        }
        //不管是那个狗，都可以叫，所以这种公共的方法，就可以写在原型中。
        Dog.prototype.bark = function () {
          console.log("汪汪汪...我的名字是"+this.name);
        }
        var d1 = new Dog("二哈","小2");
        var d2 = new Dog("泰迪","泰日天");
        d1.type = "狗"; //这是在给d1对象添加属性。
        console.log(d2.type); //undefined
        
    c.修改原型中的成员，也必须使用  构造函数.prototype  这种方式。
    
    d.如果修改原型（把原型指向另外的一个对象）；
      根据构造函数创建出来的对象，访问原型中的成员，要注意区分这个对象是 修改原型之前创建的，还是修改原型之后       创建的。（结果会不同）
          function Dog(pinZhong,name){
            this.pinZhong = pinZhong;
            this.name = name;
          }
          //不管是那个狗，都可以叫，所以这种公共的方法，就可以写在原型中。
          Dog.prototype.bark = function () {
            console.log("汪汪汪...我的名字是"+this.name);
          }
          var d1 = new Dog("二哈","小2");
          d1.bark(); //汪汪汪...我的名字是小2

          //修改Dog对相应的原型
          Dog.prototype = {
            bark: function () {
              console.log("哈哈哈");
            }
          }
          var d3 = new Dog("中华田园犬","旺财");
          d3.bark(); //哈哈哈

```



### 静态成员和实例成员

```
//静态成员：就是构造函数点出来的东西。
//实例成员：就是实例化对象点出来的东西。
  function Person(name){
    this.name = name;
  }
  Person.prototype.sayHi = function () {
    console.log("哈哈哈哈....");
  }

  //实例化对象
  var p1 = new Person("张三");
  p1.sayHi(); //这里的sayHi是实例化对象p1点出来的，所以这里的sayHi就是实例成员。

  //把Person构造函数当成是一个对象的话，往这个对象中添加方法。
  Person.sayHi = function () {
    console.log("嘿嘿额黑...构造函数当成了对象，添加的方法");
  }

  Person.sayHi(); //这里的sayHi是构造函数作为对象点出来的，所以这里的sayHi就是静态成员。
```



### 递归

```
递归 ----》函数自己调用自己 （也有结束的时候）
    var i = 0;
    function test1(){
        i++;
        console.log(i);
        console.log('哈哈');
        if(i < 5){
        	test1();
        }
    }
    test1();
```



### 回调函数

```
回调函数----》 一个函数是参数是个函数
作用----》简化代码

例子：2个参数，一个参数做了平方，一个参数加了10， 此时就有2个结果
通常我们要这样做
   function getSum(n1,n2){
     var res1 = n1 * n1;
     var res2 = n2 + 10;
     return res1 + res2;//不同的地方
   }

   function getSub(n1,n2){
     var res1 = n1 * n1;
     var res2 = n2 + 10;
     return  res1 - res2;//不同的地方
   }
  -----》发现代码冗余
  
  ---以下用回调函数方法
    function sum(num1,num2){
      return num1 + num2;
    }

    function sub(num1,num2){
      return num1 - num2;
    }

    function getRes(n1,n2, fun){
      var res1 = n1 * n1; //100
      var res2 = n2 + 10; //60
      if(f1 instanceof  Function){
        return fun(res1,res2);
      }
    }
    console.log(getRes(10, 50,  )); // 160 做加法
    console.log(getRes(10, 50, sub)); // 40 做减法
    
```



###  匿名函数 --》自调用函数

```
	//1.匿名函数 - 没有名字的函数
    //  function (){
    //    console.log("哈哈");
    //  }

    //2.匿名函数没有名字，也就意味着 ，匿名声明出来之后，没有办法用名字的方式来调用。
    //如何来调用匿名函数呢？  自己调用自己


    //3.如果一个匿名函数自己调用自己，就是自调用函数。
    //3.1
     (function (){
       console.log("哈哈");
     })();

    //3.2 推荐使用
    (function (){
      console.log("嘿嘿");
    }());


    //4.自调用函数有什么用？
    //a.如果一个函数声明出来仅仅只会被调用一次，就可以写一个自调用函数。
    //b.js中只有函数才能分割作用域， 可以用自调用函数分割作用域。
```

