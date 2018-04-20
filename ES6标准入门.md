###第2章 let和const命令

1. let命令

   1. for循环有个特别之处，循环变量是一个父作用域，循环体内部是一个单独的子作用域。

      ```javascript
      for(let i=0;i<3;i++){
          let i = "abc";
          console.log(i);
      }
      //输出结果
      //abc
      //abc
      //abc
      ```

   2. 不存在变量提升

      var命令会发生"变量提升"现象，即变量可以在声明之前使用，值为"undefined"。let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则会报错。

      ```javascript
      console.log(foo);	//undefined
      var foo = 2;

      console.log(bar);	//报错
      let bar;
      ```

   3. 暂时性死区(temporal dead zone，简称TDZ)

      只要块级作用域内存在let命令，它所声明的变量就"绑定"这个区域，不再受外部的影响。

      ```javascript
      var tmp = 123;

      if(true){
          tmp = "abc";	//报错
          let tmp;		//所谓"绑定"
      }
      ```

      TDZ就意味着 typeof 操作符不在是百分之百"安全"。

      有些死区比较隐蔽

      ```javascript
      function bar(x = y,y=2){
          return [x,y];
      }
      ```

      参数x的默认值为另一个参数y，而此时y还没有声明，属于死区。

   4. let 实际上为js新增了块级作用域。

      ```javascript
      function f1(){
          let n = 5;
          if (true) {
              let n = 10;
          }
          console.log(n);		//5
      }
      //如果使用var定义变量n，最后输出的值是10
      ```

2. 块级作用域


   1. ES5中块级作用域中声明函数变量非法。ES6引入块级作用域，在块级作用域中，函数声明语句行为类似let，在块级作用域之外不可使用。

      ```javascript
      function f(){
          console.log("I am outside!");
      }
      (function (){
          if(false){
              function f() {
                  console.log("I am inside!");
              }
          }
          f();
      }());
      ```

      该段代码在ES5中运行会得到"I am outside!".

      但是在ES6中运行会报错。

      浏览器的实现可能不遵守规范(？？？)，而有自己的行为方式：

      1. 允许在块级作用域中声明函数
      2. 函数声明类似var，即会提升到全局作用域或函数作用域头部
      3. 同时，函数声明还会提升到所在块级作用域头部

      故上述代码相当于：

      ```javascript
      function f(){
          console.log("I am outside!");
      }
      (function (){
          var f = undefined;
          if(false){
              function f() {
                  console.log("I am inside!");
              }
          }
          f();
      }());
      //Uncaught TypeError : f is not a function.
      ```

      可以使用函数表示式避免该情况。

   2. do 表达式

3. const 命令

   1. const 实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。数组，对象之类使用时应注意。
   2. 如果真想将对象冻结，应使用Object.freeze方法。

4. ES6声明变量的6种方法

   1. var和function命令
   2. let和const命令
   3. import和class命令

5. 顶层对象的属性

   1. ES5是不区分顶层对象属性和全局变量的

      ```javascript
      window.a = 1;
      console.log(a);	//1

      a = 3;
      console.log(window.a);	//3
      ```

   2. ES6逐渐将全局变量和顶部对象隔离

      为了兼容 var 和 function 命令声明的全局变量依旧是顶层对象的属性

      let，const，class命令声明的全局变量不是顶层对象的属性

### 第3章 变量的解构赋值

1. 只要某种数据结构具有Iterator接口，都可以采用数组形式的解构赋值。

2. ES6内部使用严格等于运算符判断一个位置是否有值

3. 解构对象时匹配模式

   ```javascript
   let { foo : baz } = { foo :"aaa" }
   console.log(baz);	//aaa

   let { foo } = { foo :"aaa" }
   console.log(foo);	//aaa
   ```

   ```javascript
   let obj = {
       p:[
           'Hello',{ y : "World" }
       ]
   };

   let { p:[x,{ y }] } = obj;

   console.log(x);	//"Hello"
   console.log(y);	//"World"
   ```

   此时p是模式，不是变量，因此不会被赋值，如果p也要作为变量赋值，可以写成：

   ```javascript
   let obj = {
       p:[
           'Hello',{ y : "World" }
       ]
   };

   let { p, p:[x,{ y }] } = obj;

   console.log(x);	//"Hello"
   console.log(y);	//"World"
   console.log(p);	//["Hello",{y:"World"}]
   ```

   另一个例子

   ```javascript
   var node = {
       loc : {
           start : {
               line:1,
               column:5
           }
       }
   };

   var { loc,loc:{ start },loc:{ start:{ line }}} = node;
   console.log(loc);	//{ start: { line: 1, column: 5 } }
   console.log(start);	//{ line: 1, column: 5 }
   console.log(line);	//1
   ```

   三次解构，分别对loc，start，line三个属性结构赋值，在最后一次解构中loc，start均是模式。

4. 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构

   ```javascript
   let arr = [1,2,3];
   let {0:first,[arr.length - 1] :last} = arr;

   console.log(first);	//1
   console.log(last);	//3
   ```

5. 字符串也可以解构赋值。此时字符串被转化成一个类似数组的对象。

   ```javascript
   let {length:len} = "hello";
   console.log(len);	//5
   ```

6. 函数解构时默认值

   注意两种写法的不同

   ```javascript
   function  move({x = 0,y=0} = {}) {
       return [x,y];
   }

   console.log(move({x:3,y:8}));	//[3,8]
   console.log(move({x:3}));		//[3,0]
   console.log(move({}));			//[0,0]
   console.log(move());			//[0,0]

   function  move({x,y} = {x:0,y:0}) {
       return [x,y];
   }

   console.log(move({x:3,y:8}));	//[3,8]
   console.log(move({x:3}));		//[3,undefined]
   console.log(move({}));			//[undined,undefined]
   console.log(move());			//[0.0]
   ```

7. **用途**

   1. 交换变量的值

      ```javascript
      let x = 1,y = 2;
      [x,y] = [y,x];
      console.log(x,y);	//2 1
      ```

   2. 从函数返回多个值

      ```javascript
      function example() {
          return [1,2,3];
      }

      let [a,b,c] = example();
      console.log(a,b,c);	//1 2 3
      ```

   3. 函数参数的定义

      ```javascript
      //参数是一组有次序的值
      function f([x,y,z]) { /*...*/ }
      f([1,2,3]);

      //参数是一组无次序的值
      function f({x,y,z}) { /*...*/ }
      f({z:3,y:2,x:1});
      ```

   4. 提取json数据

      **解构赋值对提取json对象中的数据尤其有用**

      ```javascript
      let jsonData = {
          id:42,
          status : "OK",
          data:[867,5309]
      };

      let {id,status,data:number} = jsonData;
      console.log(id,status,number);	//42 'OK' [ 867, 5309 ]
      ```

   5. 遍历Map结构

      ```javascript
      var map = new Map();
      map.set("first","hello");
      map.set("second","world");

      for (let [key,value] of map){
          console.log(key,value);
      }
      //first hello
      //second world
      ```

###第4章 字符串的扩展

1. 字符的Unicode表示法

   如果直接在\u后面超过0xFFFF的数值比如\u20BB7，js会理解成\u20BB+7，由于\u20BB是一个不可打印字符，所以只会显示一个空格，后面跟一个7

   ES6做出一点改进，只要将码点放入大括号中，就能正确解读该字符。即："\u{20BB7}"

2. codePointAt可以识别32位的UTF-16字符

3. fromCodePoint 方法定义在 String 对象上，而codePointAt 方法定义在字符串的实例对象上

4. 字符串遍历接口

   ```javascript
   for(let codePoint of 'foo'){
       console.log(codePoint);
   }
   //f
   //o
   //o
   ```

   这个遍历器可以识别大于0xFFFF的码点

5. padStart()，padEnd()

   padStart最常见的用途就是拿来补全指定位数

   ```javascript
   "1".padStart(10,"0");	//"0000000001"
   "12345".padStart(10,"0");	//"0000012345"
   ```

### 第6章 数值的拓展

1. Number("0b111")，Number("0o10").

2. isFinite，isNaN

   这与ES5的传统方法的区别在于，传统方法先调用Number()将非数值转为数值，再进行判断，而新方法只对数值有效，对于非数值一律返回false。isNaN只对NaN才返回true，非NaN一律返回false。

3. ES6将全局方法parseInt() 和 parseFloat() 移植到了Number对象上

4. 极小常量Number.EPSILON

5. 安全整数--正负2的53次方

6. 在V8引擎中，指数运算符与Math.pow 的实现不相同，对于特别大的运算结果，两者会有微妙的差异

7. Integer数据不能与Number类型进行混合运算

### 第7章 函数的扩展

1. 参数变量是默认声明的，所以不用let或const再次声明

   ```javascript
   function foo(x = 1){
       let x = 2;	//error
       const x = 2;//error
   }
   ```

2. 参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值。

   ```javascript
   let x = 99;
   function foo(p = x + 1) {
       console.log(p);	
   }

   foo();				//100
   x = 100;
   foo();				//101
   ```

   上面的代码中，参数p的默认值是x+1。这时，每次调用函数foo都会重新计算x+1，而不是默认p等于100

3. 函数的length属性

   1. 设置了默认值之后，函数的length属性将返回没有指定默认值的参数个数。
   2. 设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数。

4. 作用域

   一旦设置了参数的默认值，函数在进行声明初始化的时候，参数会形成一个单独的作用域。

   ```javascript
   let x = 1;
   function f(y = x) {
       let x = 2;
       console.log(y);
   }
   f();	//1
   ```

5. **应用**

   可以利用参数默认值指定某一个参数不得省略，如果省略就抛出一个错误

   ```javascript
   function throwMissing() {
       throw new Error("Missing parameter");
   }

   function foo(mustBeProvided = throwMissing()) {
       return  mustBeProvided;
   }

   foo();
   ```

6. rest参数

   1. 形式：...变量名，类似java的那个啥可变参数吧
   2. rest参数中的变量代表一个数组，所以数组特有的方法都可以用于这个变量
   3. rest参数之后不能再有其他参数

7. 严格模式

   1. 函数内部不可以使用严格模式

   2. 规避限制方法

      1. 使用全局严格模式

      2. 把函数包在一个无参数的立即执行函数里面

         ```javascript
         const doSomething  = (function () {
             "use strict";
             return function (value = 42) {
                 return value;
             };
         }());

         ```

8. name属性

   注意在赋值匿名函数时ES5和ES6的差异

   ES5返回空字符串，ES6返回实际函数名

   Function 构造函数返回的函数实例，name属性值为anonymous

   bind返回的函数，name属性值会加上 bound 前缀

9. 箭头函数

   1. 箭头函数的一个作用是简化回调函数

   2. 箭头函数体的this对象是**定义**时所在的对象，而不是使用时所在的对象

      ```javascript
      function foo() {
          setTimeout(()=>{
              console.log(this.id);
          },100);
      }
      var id =21;
      foo.call( {id:42} );	//42
      ```

   3. 箭头函数体没有自身的this，所以可以达到"绑定"this的作用

10. 箭头函数实现管道调用（QUQ没能看懂....mark一下以后看

    ```javascript
    const pipleline = (...funcs) => val => funcs.reduce((a,b) => b(a),val);
    const plus1 = a=>a+1;
    const mult2 = a=>a*2;
    const addThenMult = pipleline(plus1,mult2);

    console.log(addThenMult(5));	//12
    ```

11. 尾调用的优化

    1. 函数调用会形成一个"调用记录"，即调用帧，保存调用位置和内部变量等信息。如果A函数的内容调用函数B，那么在A的调用帧上方会形成一个B的调用帧，等到B的运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用了C就会还有一个C的调用帧。所有的调用帧就会形成一个调用栈。

    2. 尾调用由于是函数的最后一步操作，所以，不需要保存外层函数的调用帧，因为调用位置，内部变量等信息都不会再用到，**直接用内层函数的调用帧取代外层函数的即可**。

       ```javascript
       function f(){
           let m = 1;
           let n = 2;
           return g(m+n);
       }

       //等同于
       function f(){
           return g(3);
       }

       f();
       //等同于
       g();
       ```

       这就是"尾调用优化"，即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时调用帧只有一项，这将大大节省内存。

       PS:只有不在用到外层函数的内部变量的时候。内层函数的调用帧才会取代外层函数的调用帧，否则无法进行尾调用优化。

    3. 尾递归

       1. 敲黑板划重点（嘻嘻

       2. 一旦使用递归，尽量使用尾递归

       3. ES6的尾递归模式只在严格模式下才开启，正常模式是无效的

          这是因为，在正常模式下函数内部有两个变量，可以跟踪函数的调用栈。

          1. func.arguments:返回调用函数的参数
          2. func.caller:返回调用当前函数的那个函数

          尾调用优化发生时，函数的调用栈会被改写，因此上面两个变量会失真。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。

       4. 避免递归——蹦床函数

          蹦床函数可以将递归执行转化为循环执行

          ```javascript
          function trampoline(f){
              while(f && f instanceof Function){
                  f = f();
              }
              return f;
          }
          ```

          这就是蹦床函数的一个实现，可以通过蹦床函数将递归执行转化为循环执行

          ```javascript
          function sum(x,y){
              if(y > 0)
                  return sum(x+1,y-1);
              else
                  return x;
          }

          //转化为蹦床参数函数
          function sum(x,y){
              if(y>0)
                  return sum.bind(null,x+1,y-1);
              else
                  return x;
          }

          console.log(trampoline(sum(1,10000)));	//10001
          ```

          ​