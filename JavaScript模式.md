#### 第一章 简介

1. 尽量多使用对象的组合，而不是使用类的继承。

   通过已有的对象组合来获取新对象，是比通过很长的父—子继承链来创建新的对象更好的一种方法

#### 第二章 基本技巧

1. 创建隐式全局变量的反模式是带有var声明的链式赋值

   ```javascript
   function foo(){
       var a = b = 0;
       //此时，a是局部变量，b是全局变量
   }
   ```

2. 变量释放时的副作用

   隐含全局变量和明确定义的全局变量有细微的不同，不同之处在于能否使用delete操作撤销变量

   - 使用var创建的全局变量不能删除
   - 不适用var创建的隐含全局变量可以删除

   这变量隐含全局变量严格意义上来讲不是真正的变量，而是全局对象属性，属性可以通过delete操作符删除

3. 单一var模式

   只使用一个var在函数顶部进行变量声明是一种非常有用的模式，它的好处在于:

   - 提供一个单一的地址以查找到函数需要的所有局部变量
   - 防止出现变量在定义前就被使用的逻辑错误
   - 帮助牢记要声明的变量，尽可能的少使用全局变量
   - 更少的编码

   ```javascript
   function func(){
       var a = 1,
           b = 2,
           i,
           j;
   }
   ```

4. DOM操作是非常耗时的，所以注意在for循环中存储DOM容器的长度

5. for-in循环

   在循环中使用hasOwnProperty过滤原型属性

   ```javascript
   var man = {
       hands:2,
       legs:2,
       head:1
   }
   
   if (typeof Object.prototype.clone === 'undefined') {
       Object.prototype.clone = ()=>{
           console.log('colne!');
       }
   }
   
   for(var i in man){
       if(man.hasOwnProperty(i)){
           console.log(i,':',man[i]);
       }
   }
   // hands: 2
   // legs: 2
   // head: 1
   
   for (var i in man) {
       console.log(i, ':', man[i]);
   }
   // hands: 2
   // legs: 2
   // head: 1
   // clone: () => {
   //     console.log('colne!');
   // }
   ```

6. 不要增加内置的原型

7. Switch模式

   可以使用以下模式来提高switch语句的可读性和健壮性

   ```javascript
   var inspect_me = 0,
       result = '';
   switch(inspect_me){
       case 0:
           result = 'zero';
           break;
       case 1:
           result = 'one';
           break;
       default:
           result = 'unknown';
   }
   ```

   switch语句通常采用如下格式：

   - 使每个case和switch纵向排列整齐
   - 在每个case语句中使用代码缩进
   - 在每个case语句结尾有一个明确的break语句
   - 避免使用fall-throughs（也就是有意不使用break语句，以使得程序会按顺序一直向下执行）。如果确实希望采用fall-throughs，那么请确信在代码中使用fall-throughs的确是最好的途径，因为在代码中这样做会让其他阅读代码的人以为代码是有错误的（维护成本）
   - 用default语句来作为switch的结束
