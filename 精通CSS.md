1. CSS选择器的特殊性分为4个等级：a,b,c,d

   1. 如果是样式是行内样式，那么a=1
   2. b等于id选择器的总数
   3. c等于类、伪类和属性选择器的数量
   4. d等于类型选择器和伪元素选择器的数量

   **PS：伪类选择器和伪元素选择器的区别**

    	1. 伪类选择器是通过增加元素效果的选择器，不改变dom结构
   		2. 伪元素的效果是需要通过添加一个实际的元素才能达到的

2. 外边距叠加

   1. 当一个元素出现在另一个元素上面的时候，第一个元素的底外边距与第二元素的顶外边距会发生叠加
   2. 当一个元素包含在另一个元素中间时（假设没有内边距或边框将外边距分隔开），它们的顶/底外边距也会发生叠加
   3. 一个外边距甚至可以与本身发生叠加。假设有一个空元素，它有外边距，但是没有边框或内边距。在这种情况下，顶外边距与底外边距碰到一起发生自身叠加

3. 浮动与清理

   ```html
   <style>
       .new{
           background-color:gray;
           border:solid 1px black;
       }
       .new img{
           float:left;
       }
       .new p{
           float:right;
       }
       .clear{
           clear:both;
       }
   </style>

   <div class="news">
       <img src="./images/123.jpg" />
       <p>
           Some Test
       </p>
       <!--方法1:  <br class="clear" />-->
   </div>
   ```

   浮动会让元素脱离文档流，所以包围img标签和p标签的div不占空间。

   1. 可以在最后加一个空元素并且清理它，设置清理。

   2. 父元素设置overflow:hidden

      overflow属性有一个副效应，即自动清除包含的任何浮动元素

   3. 结合使用:after伪类和内容声明在指定的现有内容的末尾添加新的内容。添加内容后并把height设置为0，visibility设置为hidden。因为被清理的元素在他们的顶外边距增加了空间，所以生成的内容需要将它的display值设置为block。

4. 为了尽可能提高页面的可访问性，在定义鼠标悬停状态:hover的时候最好加上:focus伪类，在通过键盘移动到链接上的时候，让链接显示的样式与鼠标相同。

5. a标签的伪类选择器使用顺序

   ```css
   a:link,a:visited{
   	text-decoration: none;
   }
   a:hover,a:focus,a:active{
   	text-decoration: underline;
   }
   ```

   如果顺序颠倒的话，是鼠标的悬停和激活样式就不起作用

   因为a标签的同一个状态可能属于多个伪类

   访问顺序：

   a:link , a:visited , a:hover , a:focus , a:active

   详细 : https://www.cnblogs.com/xiayi/p/5350423.html

6. 不要使用a标签发送post请求

   使用锚链接而不是表单元素作为删除按钮，Google加速程序会访问这些链接以便缓存，这样以便缓存它们，这样就会无意中删除大量数据。搜索引擎的spider可以造成相同的效果，递归删除大量数据。

7. 远距离翻转

   远距离翻转是一种鼠标悬停事件，他在页面的其他地方触发显示方式的改变。实现的方法是：在锚链接内嵌套一个或多个元素；然后，使用绝对定位对嵌套的元素分别定位。尽管显示在不同的地方，但是他们都包含在一个父锚中，所以可以对同一个鼠标悬停事件作出反应。因此，当鼠标悬停在一个元素上时，可以影响另一个元素样式。

   ```html
   <style>
       .fa{
           display:block;
           position:relative;
       }
       .fa span{
           position:absolute;
           top:-1000px;	/*设置负大值移出视野*/
           left:-1000px;
       }
       .fa:hover span{		/*远距离悬停*/
           top:100px;		/*使其移动到目标位置*/
           left:100px;		/*远距离悬停注意选择器写法*/
       }
   </style>
   <a class="fa">
   	<span></span>
       <p>
           嘻嘻
       </p>
   </a>
   ```