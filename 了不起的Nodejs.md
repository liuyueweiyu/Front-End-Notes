#### PART 1

1. 再记一下一些常用JS的方法

   ```javascript
   //      1.Object.keys()
   let a = {a:"b",c:"d"};
   console.log(Object.keys(a));    //[ 'a', 'c' ]

   //      2.Array.isArray()
   console.log(Array.isArray(new Array()));

   //      3.数组方法
   [1,2,3].forEach(function (item) {   //遍历
       console.log(item);
   });
   [1,2,3].forEach(function (item) {   //filter
       return item < 2;
   });
   [1,2,3].map(i=>i*2);    //遍历赋值

   //      4.字符串方法
   console.log("   he  ".trim());  //he    去空格

   //      5.JSON
   let obj = JSON.parse('{"a":"b"}');  //转JSON
   console.log(obj.a);

   //      6. .bind
   function f() {
       console.log(this.hello == "world");
   };
   f();        //false
   let af = f.bind({hello:"world"});       //改变当前this绑定
   af();       //true

   //      7.存取器
   //      即重写getter和setter
   ```

2. 模块系统

   1. 引入模块系统（require

   2. 暴露API（module exports




#### PART 2

1. 同步和异步

   简单来说，同步就是阻塞，异步不阻塞，同步必须等这件事完成之才能继续接下来的事，但是异步就是先安排这件事去完成，然后开始另一件事，等这件事完成之后再处理这件事