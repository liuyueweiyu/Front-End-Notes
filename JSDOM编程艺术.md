1. 事件处理函数的工作机制

   在给某个元素添加事件处理函数之后，一旦事件发生，相应的JavaScript代码就会执行。被调用的JavaScript代码可以返回一个值，这个值将会被传递给那个事件处理函数。例如我们可以给某个链接添加一个onclick事件处理函数，并让这个处理函数所触发的JavaScript代码返回一个布尔值。这样一来，若返回true，onclick事件处理函数就认为"这个链接被点击了"；反之没有被点击。

   ```javascript
   <!-- 点击该链接不会发生跳页 -->
   <a href="https://www.baidu.com/" onclick="return false;">Click me!</a>
   <!-- 如果只是不想跳页还可以 -->
   <a href="#">Click me!</a>
   ```

2. nodeValue

   如果你想改变一个文本结点的值，那就使用DOM提供的nodeValue属性，它用来得到一个节点的值。

   在用nodeValue属性获取description对象的值时，得到的并不是包含在这个段落里的文本。

   ```html
       <p class="nodep">xxxx</p>
       <script type="text/javascript">
           var p =document.getElementsByClassName("nodep")[0];
           alert(p.nodeValue) ;				//null
           alert(p.childNodes[0].nodeValue) ;	 //xxxx
       </script>
   ```

   <p>元素本身的nodeValue的属性是一个空值，而你 真正需要的是<p>元素包含的文本的值

   包含在<p>元素的文本是另一种节点，它是<p>元素的第一个子节点。

3. JavaScript伪协议

   ```javascript
   javascript:alert("清除缓存");	//hhhh一行代码清除缓存
   ```

4. 关于平稳退化

   对于那些不支持或者禁用JavaScript功能的浏览器真的那么重要吗？？？（不重要，滚(╯‵□′)╯︵┻━┻

   因为有可能是爬虫orz，emmm尤其是搜索引擎的爬虫....

   所以如果不能平稳退化orz可能在搜索结果凉凉

5. 性能考虑

   1. 尽量少访问DOM和尽量减少标记

   2. 合并和放置脚本

      减少加载界面时发送的请求数量。减少请求数量通常都是在性能优化时首先要考虑的。

      脚本在标记中的位置对页面的初次加载事件也有很大影响。传统上，我们都把脚本放在文档的<head>区域，这种放置方法有一个问题。位于<head>块中的脚本会导致浏览器无法并行加载其他的文件（如图片或其他脚本）。一般来说，根据HTTP规范，浏览器每次从同一个域名中最多只能下载两个文件。而在下载脚本期间，浏览器不会下载其他任何文件，即使是来自不同域名的文件也不会下载，所有其他资源都要等脚本加载完毕后才能下载。

   3. 压缩脚本

6. 平稳退化

   ```html
   <a class="nodea" href="images/123.png">123.png</a>
   <script>
       document.getElementsByClassName("nodea").onclick = function(){
           //do something...
           return false;
       }
   </script>
   ```

   把href属性设置不过是徒手之劳，但图片库却能平稳退化~

7. 结构与行为分离的越彻底越好~

8. ​