### angular指令中的compile和link函数  
angular应用启动之前，html都是文本的形式存在，当应用启动时，会完成两个过程："编译"，"链接"。文本会编译为DOM，之后会将DOM和作用域链接在一起，最后展示拥有动态数据绑定的DOM结构。
1. 编译阶段(html文本 --> DOM节点)
> 编译阶段，angular会遍历整个文档，并根据定义的指令来解析文档中查找到的指令，这个过程是从外层到内层的查询，这个编译过程不会停止，直到遍历到最后一层的最后一个指令。在这个编译完成后，链接阶段之前，可以修改编译好的DOM节点，但还不能没有将作用域绑定，所以不能访问scope。

  ```js
  return {
    compile: function(ele, attrs, transclude){
      //TODO
      //必须返回link之后的回调link函数
      return function(scope,iele,iattrs,controller){

      }
      //或者是link函数之前的pre和之后的post函数
      return {
        pre: function(scope,iele,iattrs,controller){

        },
        post: function(scope,iele,iattrs,controller){

        }
      }
    }
  }
  ```
 >ele代表指令根节点元素的jqLite对象(轻量的jq对象，如果存在jq，则被替换为jq对象);  
 attrs表示指令中的属性，如attrs.ngModel,attrs.class等;  
 transclude表示是否在指令中插入或者变化内容

2. 链接阶段(DOM节点绑定作用域scope)
> 链接阶段，angular会将作用域scope与DOM节点链接，这个阶段会将scope注入。  
上述的编译阶段中可以返回link函数(compile存在时必须的)，返回的link函数则会覆盖点单独写的link函数

 ```js
  return {
    link: function(scope, iele, iattrs, controller){
      //这里的函数会被compile返回的link覆盖点
    }
  }
 ```
 > 当link完成之后，DOM就会被展示在界面中。  

对于compile和link的区别:
> 1. compile只是对指令模板的转换。
2. link是在数据和视图之间建立联系，设置事件的监听等。
3. scope是在link阶段链接被绑定到元素上的，compile无法访问到。
4. 对于同一个指令的多个实例，compile只会被执行一次，link对每一个实例都会执行一次。
5. compile函数必须返回link函数做后续处理。
