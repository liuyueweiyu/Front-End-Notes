---
typora-copy-images-to: images
---

1. propTypes

   propTypes是个对象，可以为每个传入组件的属性添加类型检查

2. statics

   在组件规范中，可以在静态(statics)属性中设置静态函数的对象。你的静态函数在组件中运行，并且可以在函数所创建的实例中调用

3. displayName

   displayName是在你查看你的Reack应用的调试信息时所使用的属性

4. **生命周期**

   1. componentWillMount

      componentWillMount是生命周期时间，React在拿你的组件类渲染成DOM的过程中使用它。componentWillMount方法仅在你的组件初始渲染前执行。有一件需要特别注意的事，**如果你在该函数中调用你的setState函数，componentWillMount不会使你的组件重新渲染，因为初始渲染方法会恢复被改变的状态。**（这句话的重点感觉是不会使其重新渲染？？）

   2. componentDidMount

      只在客户端的React进程中，在组件被渲染成DOM后使用，在这个点上，react组件已经成为DOM中的一部分，可以使用React.findDOMNode函数调用它。

   3. componentWillReceiveProps

      每次有属性值改变该函数就会被执行，但第一次渲染的时候决不会执行，可以在这个函数中调用setState，这样不会导致**额外**的渲染。该函数的参数为新对象的，它**将**成为组件属性的一部分，并且仍然可以通过this.props访问当前属性，这样就可以在该函数中为this.props和nextProps做任意逻辑比较

      ```javascript
      componentWillReceiveProps(nextProps){
          //...
      }
      ```

   4. shouldComponentUpdate

      该函数在组件渲染前被调用，**每次属性改变或者状态被恢复**都会被调用。在初始渲染前或者是使用forceUpdate时不会被调用。这个函数可以判断属性改变或者状态改变是不是真的需要更新时跳过渲染的机制。这样会绕开组件渲染，不仅是跳过了render函数，还会到生命周期的下一步，即componentWillUpdate和competentDidUpdate

      ```javascript
      shouldComponenttUpdate(nextProps,nextStates){
          //...
          return bool;	//决定要不要渲染
      }
      ```

   5. componentWillupdate

      在你的组件一出现渲染时就被调用。**无法在该函数中使用setState**

   6. componentDidupdate

      仅在DOM的**所有**渲染更新被处理完后被执行。因为它基于更新，所以不是组件初始渲染的一部分。可给该函数的参数是之前的属性和之前的状态

      ```javascript
      componentDidUpdate(prevProps,prevStates){
          //...
      }
      ```

   7. componentWillUnmount

      该函数不再在DOM上mount时会被立马调用

   PS：关于生命周期可以结合这张图

   ​	![img](https://user-gold-cdn.xitu.io/2017/11/11/88e11709488aeea3f9c6595ee4083bf3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

   或者看看这篇[博客](https://juejin.im/post/5a062fb551882535cd4a4ce3)

5. 以大写字母做为组件开头，html标签以小写开头

