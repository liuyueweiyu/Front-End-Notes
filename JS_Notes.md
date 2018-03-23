### JS学习笔记

1. 浏览器遇见字符串"</script>"就会认为是结束标签，想要输出"</script>"字符串需要转义，即输出"<\/script>"

2. 在严格模式下，不能定义名为eval或arguments的变量，否则会导致语法错误 (待解决)

3. 如果定义的变量准备在将来用来保存对象，那么最好将它初始化为null

   并且 undefined 值派生自 null 值

   null == undefined //true

4. 注意隐式调用Boolean()函数

5. ​