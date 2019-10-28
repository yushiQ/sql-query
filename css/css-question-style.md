## css 高频问题详解

##### 1、 BFC 是什么，如何触发 BFC

###### FC

Formatting Context（格式化上下文）是 W3C 规范中的一个概念，
他是页面中的一块喧染区域，并且有一套喧染规则，他决定了其子元素如何定位，以及和其他元素的关系和相互作用，

###### BFC

BFC：块级元素格式化上下文
IFC：内联元素格式化上下文（面试不常考）

BFC 即 Block Formatting Contexts（块级格式化上下文），他属于上述定位方案的普通流。

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

###### 触发 BFC

###### 只要满足以下任意一条件，将会触发 BFC

- body 根元素
- 浮动元素 ： float ：none 以外的值
- 绝对定位元素 ： position ：absolute / fixed
- display :inline-block / table-cell /flex
- overflow : 除了 visible 以外的值（hidden / auto / scroll）

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

##### 2、 position: sticky

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

###### IE 怪异盒模型

```
  而IE盒模型或怪异盒模型显而易见的区别就是，width和height除了content区域外，还包含padding和border。盒子的总宽度为

  一个块的总宽度= width + margin(左右)（即width已经包含了padding和border值）
```

###### 如何运用和选择哪种盒模型

    首先，如何运用？

    只要在文档首部加了doctype申明，即使用了标准盒模型，而不加，则会由浏览器自己决定，比如，ie 浏览器中显示“ie盒子模型”，在 ff 浏览器中显示“标准 w3c 盒子模型”。

    然后，选择哪种？

    按理说，我们应该遵循w3c标准使用标准盒模型，但我发现很多ui框架使用的都是怪异盒模型，比如ionic,vux,和bootstrap,为什么呢?我思考了很长时间。

    不知道大家有没有遇到过这种情况，本来我画了两个div,他们在flex布局下分别占50%的宽度，很简单，我们width设置为50%即可，当我们画完了，很满意的看了看页。

    代码:

    * {
    margin: 0;
    padding: 0;
    box-sizing: content-box;

}
<div style="display: flex;">
<div style="width: 50%;background-color: blue;color: white;">我是左边</div>
<div style="width: 50%;background-color: red;color: white">我是右边</div>
</div>
效果：

    这时候，一个需求来了，左边的部分要加上边框,你一想，这不简单么，立马给左边加上了一个边框。但，意外出现了，左边和右边的不相等了。因为左边加上了边框，所以宽度溢出了，这时候右边就会压缩自己，这样就导致两边不一致了

    代码：

    <div style="display: flex;box-sizing: content-box">
      <div style="width: 50%;background-color: blue;color: white;border: 5px solid black">我是左边</div>
      <div style="width: 50%;background-color: red;color: white">我是右边</div>
    </div>
    效果：

    因为border必须为数值，而其他的为百分比，这样就很难知道左右该设多少比例，才能让左右两边完全一致了。

    而用怪异盒模型，这样的问题就迎刃而解。

    代码：

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

   <div style="display: flex;">
       <div style="width: 50%;background-color: blue;color: white;border: 5px solid  black"> 我是左边</div>
       <div style="width: 50%;background-color: red;color: white">我是右边</div>
   </div>

    这样不管你边框是多宽，左右都是始终相等的。

    然后，再请大家想一想，移动端都是需要自适应布局的，需要用到大量的百分数，这时候再用标准盒模型，边框进来插一脚无疑会让情况变得很复杂，对自适应布局很不友好，这大概就是很多框架都采用怪异盒子模型的原因吧

##### 4、PostCss 它本质上是一个什么东西

      1、PostCSS 可以直观的理解为：它就是一个平台
      2、PostCSS 提供了一个解析器，它能够将 CSS 解析成抽象语法树（AST）

      我们编写的 css=> css parser => plugin system => stringifier => 最终的css

      PostCSS 它需要一个插件系统才能够发挥作用

###### 它能解决我们什么问题？它是通过什么方式来解决我们的问题？

    它能够为 CSS 提供额外的功能；

    通过在 PostCSS 这个平台上，我们能够开发一些插件，来处理我们的CSS，比如热门的：autoprefixer

    我们能够使用JavaScript来开发插件（这点对前端来说很重要）

    // postcss 的命令行工具
    sudo npm install  -g postcss-cli
    // autoprefixer 插件
    sudo npm install -g autoprefixer

    第一次用命令行能让你更直观的去理解

```
    // 1. 先看下这个命令有哪些参数可以用
    postcss --help

    Usage: /usr/local/bin/postcss -use plugin [--config|-c config.json] [--output|-o
    output.css] [input.css]

    选项：
      -c, --config       JSON file with plugin configuration
      -u, --use          postcss plugin name (can be used multiple times)
      -o, --output       Output file (stdout if not provided)
      -d, --dir          Output directory
      -r, --replace      Replace input file(s) with generated output       [boolean]
      -s, --syntax       Alternative input syntax parser
      -p, --parser       Alternative CSS parser
      -t, --stringifier  Alternative output stringifier
      -w, --watch        auto-recompile when detecting source changes
      -v, --version      显示版本号                                        [boolean]
      -h, --help         显示帮助信息                                      [boolean]

    示例：
      postcss --use autoprefixer -c             Use autoprefixer as a postcss plugin
      options.json -o screen.css screen.css
      postcss --use autoprefixer                Pass plugin parameters in
      --autoprefixer.browsers "> 5%" -o         plugin.option notation
      screen.css screen.css
      postcss -u postcss-cachify -u             Use multiple plugins and multiple
      autoprefixer -d build *.css               input files

    Please specify at least one plugin name.
```

###### 它解决我们的问题是为什么？优势何在？

比如，我们用 SASS 来处理 box-shadow 的前缀，我们需要这样写：

    /* CSS3 box-shadow */
    @mixin box-shadow($top, $left, $blur, $size, $color, $inset: false) {
        @if $inset {
            -webkit-box-shadow: inset $top $left $blur $size $color;
            box-shadow: inset $top $left $blur $size $color;
        } @else {
            -webkit-box-shadow: $top $left $blur $size $color;
            box-shadow: $top $left $blur $size $color;
        }
    }

使用 PostCSS 我们只需要按标准的 CSS 来写就行了，因为最后 autoprefixer 会帮我们做添加这个事情～

    box-shadow: 0 0 3px 5px rgba(222, 222, 222, .3);
    所以，这里就出现了一个经常大家说的未来编码的问题。实际上，PostCSS 改变的是一种开发模式。

    SASS等工具：源代码 -> 生产环境 CSS

    PostCSS：源代码 -> 标准 CSS -> 生产环境 CSS

    这样能体会出优势吧，但是目前大家都是 SASS + PostCSS 这样的开发模式，其实我认为是不错的，取长补短嘛，当然，在 PostCSS 平台上都是可以做到的，
    只是目前这个过渡期，这样更好，更工程化。接下来我就介绍一些方法来纯粹是用 PostCSS。

它由哪些东西组成？
其实从官方介绍来看，只包含以下内容：

        CSS Parser

        CSS 节点树 API

        source map 生成器

        生成节点树串

地址 https://segmentfault.com/a/1190000003909268

##### 5、如何实现一个自适应的正方形

元素的 padding 或 margin 值是百分比值，那么，它的值是根据父元素的宽度来计算的。

```
1、
 <style>
     .box{
         width: 50%;
         padding-top: 50%;
         background-color: black;
     }
 </style>
 <div class="box"></div>
2、
   <style>
         .box{
              overflow: hidden;
           width: 50%;
            background-color: black;
         }
       .margin{
            margin-top: 100%;
         }
     </style>
    <div class="box">
       <div class="margin"></div>
    </div>
```
