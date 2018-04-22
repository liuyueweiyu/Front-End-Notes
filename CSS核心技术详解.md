#### 第1章 遇见未知的CSS

1. 使用pointer-events控制鼠标事件

   ```css
   .disabled{
   	pointer-events:none;	/* 禁用鼠标事件 */   
   }
   ```

   还可以将一个遮罩元素的属性设置为poiner-events:none;这样就可以单击到遮罩层后面的元素。

   如果将其设置为auto，那么单击子元素是还是会通过事件冒泡的形式触发父元素的事件

2. 玩转CSS选择器（是的超级好玩！！！

   - 当父元素下只有一个元素才会被选中

     ```css
     div:first-of-type:last-of-type{
         color:red;
     }
     /*或者*/
     div:only{
         color:red;
     }
     ```

   - 当父元素下有多个子元素时，选中第一个

     ```css
     div:not(:last-of-type):first-of-type{
         color:red;
     }
     /*或者*/
     div:not(:only-child){
         color:red;
     }
     ```

   - 可以结合使用上面两个XDDD

3. 利用padding实现元素的等比例缩放

   padding和margin有个奇怪（x的特点，它们的上下外边距的百分比是根据父元素的宽度计算的

   ```html
   <style>
   .box{
       width: 100px;
       height: 10px;
   }
   .box div{
       position: relative;
       overflow: hidden;
       background-color: red;
   }
   .box div::after{
       content: "";
       width: 100%;
       display: block;
       padding-top: 100%;

   }
   .box div span{
       position: absolute;
       left: 0;
       top: 0;
   }
   </style>

       <div class="box">
           <div>
               <span>图片</span>
           </div>
       </div>
   ```

   利用伪元素的padding-top"撑开"父元素的高，内容通过绝对定位来使用，因为绝对定位的元素是不占位置的，这样一个等比例宽高缩放就完成了。

4. calc函数

   1. 运算符前后都需要保留一个空格

5. 隐藏元素

   - width或height为0

   - opacity为0：元素本身存在只是看不见而已

   - 通过定位将元素移出屏幕

     ```css
     div{
         position:absolute;
         left:-9999px;
     }
     ```

   - text-indent实现文字隐藏

     ```css
     div{
         text-indent:-9999px;
     }
     ```

     给页面添加logo图片，若还想被搜索引擎搜索到，则需要添加这段文字，但是如果不想显示这段文字就可以使用这个方法。

   - 通过z-index隐藏一个元素

   - 设置overflow:hidden

   - 设置visibility:hidden：元素不可见，但还占位置

   - 设置display:none：将元素彻底隐藏

   - 将元素背景设置为透明，字体大小设置为0

     ```css
     div{
         font-size:0;
         background-color:transparent;	/*元素还在只是不可见*/
     }
     ```

   - 将元素的max-width或者max-height设置为0

     宽度为0还会有文字溢出的问题，解决这个问题可以设置overflow:hidden;将文字大小设置为0。

     不过单纯的设置max-width为0的话可以实现**文字竖排**的效果唉！英文的话还得加上一个换行属性：word-break:break-all;

   - transform的translate函数隐藏一个元素

     ```css
     div{
         transform:translate(-999999px);	/*移出可视区域*/
     }
     ```

   - 将元素缩放设置为0

     ```css
     div{
         transform:scale(0);
     }
     ```

   - 让元素重叠

     ```css
     div{
         transform:skew(90deg);	/*类似width等于0*/
     }
     ```

   - 设置margin为负大值

#### 第2章 CSS核心概念

1. 格式化上下文BFC，IFC

   1. 从overflow清除浮动看BFC

      overflow处理方式：如果这个元素没有设置宽高，那么就将所有元素的宽高加起来做父盒子的宽高，就导致了永远不可能溢出。所以，用overflow清除浮动起只不过是让父元素重新自适应了子元素的宽高，顺便清除浮动。

   2. 从overflow看BFC

      ================================================================================

      = =！这个没看的很懂唉！感觉回头再看一遍把！