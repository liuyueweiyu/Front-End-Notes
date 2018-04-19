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