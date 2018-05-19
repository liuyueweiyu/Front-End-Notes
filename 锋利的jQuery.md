1. jQuery对象和DOM对象的相互转换

   ```javascript
   //jQuery对象转成DOM对象
   let box = $(".div");
   console.log(box instanceof jQuery);	//true
   console.log(box[0] instanceof jQuery);	//false

   //DOM对象转成jQuery对象
   let box = document.createElement("div");
   console.log(box instanceof jQuery);	//false
   console.log($(box) instanceof jQuery);	//true
   ```

2. 基本过滤选择器

   | 选择器         | 描述                           | 示例                                                |
   | -------------- | ------------------------------ | --------------------------------------------------- |
   | :first         | 选取第一个元素                 | $("div:first")                                      |
   | :last          | 选取最后一个元素               | $("div:last")                                       |
   | :not(selector) | 去除所有给定选择器匹配的元素   | $("div:not(.myClass)")选取class不为myClass的div元素 |
   | :even          | 选取索引是偶数的所有元素       | $("div:even")                                       |
   | :odd           | 选取索引是奇数的所有元素       | $("div:odd")                                        |
   | :eq(index)     | 选取引索等于index的元素        | $("div:eq(0)")                                      |
   | :gt(index)     | 选择索引大于index的所有元素    | $("div:gt(1)")                                      |
   | :lt(index)     | 选择索引小于index的所有元素    | $("div:lt(1)")                                      |
   | :header        | 选取所有标题元素,h1,h2...      | $("div:header")                                     |
   | :animated      | 选取当前正在执行动画的所有元素 | $(":animated")                                      |
   | :focus         | 获取当前获得焦点的元素         | $(":focus")                                         |

3. 内容过滤选择器

   内容过滤器的过滤规则主要体现在它所包含的子元素或文本内容上

   | 选择器         | 描述                           | 示例                   |
   | -------------- | ------------------------------ | ---------------------- |
   | :contain(text) | 选取含有文本内容为"text"的元素 | $("div:contain('我')") |
   | :empty         | 选取空元素                     | $("div:empty")         |
   | :has(selector) | 选取含有选择器的元素的元素     | $("div:has(.myClass)") |
   | :parent        | 选取含有子元素或者文本的元素   | $("div:parent")        |

4. 属性过滤选择器

   | 选择器                       | 描述                                    | 示例                     |
   | ---------------------------- | --------------------------------------- | ------------------------ |
   | [attribute]                  | 拥有此属性的元素                        | $("div[id]")             |
   | [attribute=value]            | 所选属性为value的元素                   | $("div[name=test]")      |
   | [attribute!=value]           | 所选属性不为value的元素                 | $("div[name!=test]")     |
   | [attribute^=value]           | 所选属性的值以value开头的元素           | $("div[title^=test]")    |
   | [attribute$=value]           | 所选属性的值以value结尾的元素           | $("div[title\$=test]")   |
   | [attribute*=value]           | 所选属性的值含有value的元素             | $("div[title*=test]")    |
   | [attribute\|=value]          | 所选元素已value为前缀的元素             | $("div[title\|=test]")   |
   | [attribute~=value]           | 所选属性用空格分隔的值中包含value的元素 | $("div[title~=test]")    |
   | \[attribute1][attribute2]... | 复合属性                                | $("div\[id][name=test]") |

5. 表单对象属性过滤选取器

   - :enabled 选取所有可用的元素
   - :disabled 选取不可用元素
   - :checked 选取所有被选中的元素（单选框，复选框
   - :selected 选取所有被选中的选项元素（下拉框

6. 表单选择器

   | 选择器    | 描述                                        |
   | --------- | ------------------------------------------- |
   | :input    | 选取所有input，textarea，select，button元素 |
   | :text     | 选取所有的单行文本框                        |
   | :password | 选取所有密码框                              |
   | :checkbox | 选取所有多选框                              |
   | :submit   | 选取所有提交按钮                            |
   | :image    | 选取所有图像按钮                            |
   | :reset    | 选取所有重置按钮                            |
   | :button   | 选取所有的按钮                              |
   | :file     | 选取所有的上传域                            |
   | :hidden   | 选取所有不可见元素                          |

7. 选择器中含有特殊字符

   选择器中含有 . # ( [ 记得转义

8. filter 筛选元素

9. 某节点remove方法删除后，该节点所包含的所有后代节点将同时删除

10. detach也是删除节点，不过与remove不同的是，所有绑定的事件和附加的数据都会被保留下来

11. empty方法为清空该元素里的所有内容，以及子代

12. clone方法，复制节点，clone方法中传递一个参数true，它的含义是复制元素的同时复制元素中的绑定事件

13. replaceWith替换元素方法，如果在替换之前，已经为元素绑定事件，替换后原先绑定的事件将会与被替换的元素一起消失，需要在新的元素上重新绑定事件

14. 包裹节点

    ​