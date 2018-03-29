### JS学习笔记

1. 浏览器遇见字符串"</script>"就会认为是结束标签，想要输出"</script>"字符串需要转义，即输出"<\/script>"

2. 在严格模式下，不能定义名为eval或arguments的变量，否则会导致语法错误 (待解决)

3. 如果定义的变量准备在将来用来保存对象，那么最好将它初始化为null

   并且 undefined 值派生自 null 值

   ```javascript
   null == undefined 	//true
   ```

4. 注意隐式调用Boolean()函数

5. NaN和任何值都不相等，包括NaN本身

6. isNaN()也适用于对象

   基于调用对象时，会首先调用对象的valueOf() 方法，确定该返回的值能否转化为数值，如果不能，将会基于这个返回值，调用toString()返回值，再测试返回值

7. Number() 函数转换

   null 返回 0

   undefined 返回 NaN

   对象 同tip6

8. parseInt() 函数转换

   从头解析至第一个非数字字符的数值

   ```javascript
   parseInt("1234bule")；	//1234
   ```

   空字符串会返回NaN

9. parseFloat()只解析十进制

10. 字符串解析特点

    ES 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。

    ```javascript
    var lang = "Java";
    lang = lang + "Script";
    ```

    变量lang开始时包含字符串"Java"。然后创建一个能容纳10个字符的新字符串，然后在这个字符串中填充"Java"和"Script"，最后在将原来的"Java"和"Script"字符串销毁

11. Object 类型

    ```javascript
    var o = new Object();

    o.constructor();	//构造函数
    o.hasOwnProperty(propertyName);	//用于检查给定的属性在当前对象的实例中是否存在，propertyName必须以字符串形式指定
    o.isPrototypeOf(object);	//用于检查传入的对象是否是当前对象的原型
    o.propertyIsEnumberable(propertyName);	//用于检查所给属性是否能够迭代
    o.toLocaleString();		//返回对象的字符串表示，与执行环境的地区有关
    o.toString();	//返回字符串表示
    o.valueOf();	//返回对象的字符串、数值或布尔值
    ```

12. Infinity 加 -Infinity 结果为NaN

13.  == 和 ===

    **==**	如果两个操作数相等返回true，不相等返回false。这两个先转换操作数(通常称为**强制转型**)，然后再比较相等性

    ​	如果一个数是布尔值，在比较之前将其转换为数值

    ​	一个是字符串，一个是数值，比较之前将字符串转化为数值

    ​	两个操作数都是对象，则比较它们俩是不是同一个对象

    **===**	它只在两个操作数未经过转换就不相等的情况下才返回true

14. for-in 语句

    ES 对象的属性没有顺序。因此，通过for-in循环输出的属性名的顺序是不可预测的。具体来说，所有属性都会被返回一次，返回的顺序可能因浏览器而异。

15. label 语句

    ```javascript
    var number = 0;
    label:
    	for(;;)
            for(var i = 0;i<10;i++){
                if(i==5)
                    break label;
                number++;
            }
    console.log(number);	//5
    ```

16. with 语句

    ```javascript
    var qs = location.search.substring(1);
    var hostname = location.hostname;
    var url = location.href;

    //上述语句等价于
    with(location){
        var qs = search.substring(1);
    	var hostname = hostname;
    	var url = href;
    }
    ```

17. 函数参数

    EMACScript 的函数是不介意传递多少个参数的。函数体内可以通过arguments对象来访问参数数组的，命名的参数只是提供便利，不是必须。

    关于arguments的行为。它的值永远与对应命名的参数的值保持同步。

    ```javascript
    function test(name){
        console.log(name);			
        console.log(arguments[0]);	 
        console.log(arguments[1]);	 
    }

    test(1);
    //1 1 undefined

    test(1,2);
    //1 1 2
    ```

    形参name的值和arguments[0]的值保持同步，但是并不是说它们共有同一块内存空间，它们的内存空间相互独立，但是它们的值保持同步。

    此外，如果**传入**一个参数，那么arguments[1] 设置的值不会反应到命名参数中，因此arguments的参数的长度只和传入参数的个数有关。

    严格模式对arguments对象作出了一部分限制，形参的值和arguments数组的值不再保持同步。

    ```javascript
    "use strict";
    function test(name) {
        console.log(name);				// 1
        arguments[0] = 10;				
        console.log(name);				// 1
        arguments[0] = name;
        name = 10;
        console.log(name);				// 10
        console.log(arguments[0]);		 //1
    }
    test(1);
    ```

    第4行中，arguments[0]的值被修改后，非严格模式下name的值也会被修改，非严格模式下会输出10

18. ES 变量 : **基本类型值**和**引用类型值**

    其中，string 是基本类型值，不以对象进行表示。

    ```javascript
    var name = "test";
    name.age = 27;
    console.log(name.age);	//undefined
    ```

    定义基本类型和引用类型的变量都是在栈内存中分配值，但是基本类型的值是在栈中，而引用类型的值在堆中，栈中保存的对应在堆中的地址。当复制某个基本类型值的时候是在栈中开辟一段空间，保存其变量和其值，复制引用类型的变量时，则是在栈中开辟一段空间保存其变量和地址。所以修改经过复制得到的引用类型变量是会影响被复制的变量，而基本类型不会。

    ![img](C:\Users\官欣仪\Desktop\hahaha\Front-End Notes\images\201708250850131.png)

    ​

    ![img](C:\Users\官欣仪\Desktop\hahaha\Front-End Notes\images\201708250850132.png)

    在传递参数时，基本类型变量和引用类型变量传递的都是值

    ```javascript
    function setName(obj){
        obj.name = "name1";
        obj = new Object();
        obj.name = "name2";
    }
    var person = new Object();
    setName(person);
    console.log(person.name);	//name1
    ```

    如果是按引用传递的话，在setName函数体中，person会自动指向name为"name2"的新对象。但是再次访问person.name时，显示值为"name1"。这说明即使在函数内部修改了参数的值，但是原始的引用并没有改变。

    故，善用 typeof 和 instanceof

    ```javascript
    var a = new object();
    var b = 11;
    console.log(typeof a);	// object
    console.log(typeof b);	// number
    console.log(a instanceof Object);	//true
    console.log(b instanceof Object);	//false	
    ```

19. 环境，作用域，作用域链 = = 这玩意自己看书吧...

    环境

    变量对象

    环境中的所有变量和函数都在变量对象中


    每个函数都有自己的执行环境
    
    
    当代码在环境中执行时，会创建变量对象的一个作用域链
    
    凡是搜索啊什么什么之类的，基本都是从作用域链开始从前往后搜，作用域链最开始是arguments对象，然后再一层一层往外套。

20. with语句,try...catch...语句可以延长作用域链(IE9好像修复了trycatch的这个问题...没测试过不知道)

21. js没有块级作用域

    ```javascript
    for(var i = 0;i<10;i++){
        //...
    }
    console.log(i);	//10
    ```

22. 如果初始化变量时没有用var声明，该变量会自动被添加到全局环境。

    严格模式下，这样操作会报错。

23. 接触一个值的引用并不是意味着自动回收该值所占用的内存，解除引用的真正作用是让值脱离执行环境，以便垃圾收集器在下次运行时将其回收。

24. 迭代器

    every()	对于给定函数每一项返回true则返回true

    filter()	对于给定函数，返回true项组成的数组

    forEach() 遍历运行给定函数，没有返回值

    map()	返回每次运行的结果

    some()	对于给定函数存在返回true则返回true

    ```javascript
    var numbers = [1,2,3,4,5,4,3,2,1];

    function moreThanTwo(number) {
        return (number>2);
    }

    console.log(numbers.every(moreThanTwo));	//ture
    console.log(numbers.filter(moreThanTwo));	//[ 3, 4, 5, 4, 3 ]
    console.log(numbers.forEach(moreThanTwo));	//undefined
    console.log(numbers.map(moreThanTwo));		//[ false, false, true, true, true, true, true, false, false ]
    console.log(numbers.some(moreThanTwo));		//true
    ```

25. Object 对象

    数据属性

    configurable

    ```javascript
    var person = {
        name : "name1"
    }
    Object.defineProperty(person,"name",{
        configurable : false,	//设置不可修改
        value:"name2"
    })

    console.log(person.name);	//	name2
    Object.name = "name3";	
    console.log(person.name);	//	name2
    Object.defineProperty(person,"name",{
        configurable : true		//	设置为false后再设置为true程序报错。
    });
    console.log(person.name)
    ```

    访问器属性

    在读取访问器属性的时候，会调用getter函数，这个函数返回属性的值，也可以通过setter函数，所以可以通过重写get，set函数来进行一些新的操作

    ```javascript
    var book = {
        _year : 2014,   //变量前加_表示只能通过对象返问的属性
        year : 2000,
        edition : 1
    };
    console.log(book._year);	//2014
    console.log(book.year);		//2000
    console.log(book.edition);	//1

    Object.defineProperty(book,"year",{
        get:function () {
            return this._year;
        },
        set:function (newValue) {
              this._year = newValue;
              this.edition += newValue - 2014;
        }
    });
    book.year = 2015;
    console.log(book._year);	//2015
    console.log(book.year);		//2015
    console.log(book.edition);	//2
    ```

    ​