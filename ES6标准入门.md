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

