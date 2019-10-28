## css高频问题详解

#####   1、 BFC是什么，如何触发BFC
######  FC
Formatting Context（格式化上下文）是W3C规范中的一个概念，
他是页面中的一块喧染区域，并且有一套喧染规则，他决定了其子元素如何定位，以及和其他元素的关系和相互作用，
######  BFC
BFC即Block Formatting Contexts（块级格式化上下文），他属于上述定位方案的普通流。

具有BFC特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且BFC具有普通容器所没有的一些特性。

通俗一点来讲，可以把BFC理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

###### 触发BFC
######   只要满足以下任意一条件，将会触发BFC
*  body根元素
*  浮动元素 ： float ：none以外的值
*  绝对定位元素 ： position ：absolute / fixed
*  display :inline-block / table-cell /flex
*  overflow : 除了visible 以外的值（hidden / auto / scroll）

###### 解决方法
  清除浮动 
  
    1、  加伪类

         .box:after{
             content:"";
             width:0;
             height:0;
             clear: both / right / left;
             overflow:hidden;
             display：block;
             -zoom:1  (兼容IE6)
         }
    
    2、  父元素脱离文档流

          加定位，父元素浮动 ，除了 position: relative /static / float:none      
#####   2、  position: sticky
```
 这是一个结合了 position:relative 和 position:fixed 两种定位功能于一体的特殊定位，适用于一些特殊场景。

position:sticky的生效是有一定的限制的，总结如下：

1、须指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。并且top和bottom同时设置时，top生效的优先级高，left和right同时设置时，left的优先级高。

2、设定为position:sticky元素的任意父节点的 overflow 属性必须是 visible，否则position:sticky不会生效。这里需要解释一下：

   1.如果position:sticky元素的任意父节点定位设置为overflow:hidden，则父容器无法进行滚动，所以position:sticky元素也不会有滚动然后固定的情况。

   2.如果position:sticky元素的任意父节点定位设置为position:relative | absolute | fixed，则元素相对父元素进行定位，而不会相对 viewprot 定位。

   3.达到设定的阀值。也就是设定了position:sticky的元素表现为relative还是fixed是根据元素是否达到设定了的阈值决定的。
```
##### 3 、标准盒模型和怪异盒模型
###### 标准盒模型
```
   1、在标准盒模型下，width和height是内容区域即content的width和height。而盒子总宽度为

   2、在标准模式下，一个块的总宽度= width + margin(左右) + padding(左右) + border(左右)
```
###### IE怪异盒模型
```
  而IE盒模型或怪异盒模型显而易见的区别就是，width和height除了content区域外，还包含padding和border。盒子的总宽度为

  一个块的总宽度= width + margin(左右)（即width已经包含了padding和border值）
```
###### 如何运用和选择哪种盒模型