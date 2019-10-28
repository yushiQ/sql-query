##### 1、CMD、AMD、CommonJS、UMD 规范分别指什么

    AMD (Asynchronous Module Definition, 异步模块定义) 指定一种机制，在该机制下模块和依赖可以移步加载。这对浏览器端的异步加载尤其适用，其也是CommonJS规范的一个延伸。

AMD 的库有 RequireJS 、curl 、Dojo 等。CommonJS 是服务器端模块的规范，Node.js 采用了这个规范。Node.JS 首先采用了 js 模块化的概念。在一个模块中，存在一个自由的变量”require”，它是一个函数。这个”require”函数接收一个模块标识符。“require”返回外部模块所输出的 API。如果出现依赖闭环(dependency cycle)，那么外部模块在被它的传递依（transitivedependencies）所 require 的时候可能并没有执行完成；在这种情况下，”require”返回的对象必须至少包含此外部模块在调用 require 函数（会进入当前模块执行环境）之前就已经准备完毕的输出。如果请求的模块不能返回，那么”require”必须抛出一个错误。在一个模块中，会存在一个名为”exports”的自由变量，它是一个对象，模块可以在执行的时候把自身的 API 加入到其中。模块必须使用”exports”对象来做为输出的唯一表示。

    CMD（Common Module Definition）是 SeaJS推广过程中产生的。

CMD 由国内的玉伯提出，其与 AMD 规范的主要区别在于定义模块，和依赖引入部分。AMD 需要在声明模块的时候指定所有的依赖，通过形参传递到模块内部

与 AMD 模块相比，CMD 更接近与 Node 对 CommonJS 规范的定义

UMD 叫做通用模块定义规范（Universal Module Definition）。也是随着大前端的趋势所诞生，它可以通过运行时或者编译时让同一个代码模块在使用 CommonJs、CMD 甚至是 AMD 的项目中运行。未来同一个 JavaScript 包运行在浏览器端、服务区端甚至是 APP 端都只需要遵守同一个写法就行了。

它没有自己专有的规范，是集结了 CommonJs、CMD、AMD 的规范于一身，我们看看它的具体实现：

    ((root, factory) => {
        if (typeof define === 'function' && define.amd) {
            //AMD
            define(['jquery'], factory);
        } else if (typeof exports === 'object') {
            //CommonJS
            var $ = requie('jquery');
            module.exports = factory($);
        } else {
            root.testModule = factory(root.jQuery);
        }
    })(this, ($) => {
        //todo
    });

不难发现，它在定义模块的时候回检测当前使用环境和模块的定义方式，将各种模块化定义方式转化为同样一种写法。它的出现也是前端技术发展的产物，前端在实现跨平台的道路上不断的前进，UMD 规范将浏览器端、服务器端甚至是 APP 端都大统一了，当然它或许不是未来最好的模块化方式，未来在 ES6+、TypeScript、Dart 这些拥有高级语法的语言回代替这些方案

##### 2、gulp

    gulp是基于流的自动化构建工具，
    gulp的核心就是一个gulpfile.js的文件，其中包含了自定义的gulp任务（task），
    在命令行运行gulp指令时，会执行其中的指令。

    gulp可以进行js，html，css，img的压缩打包，是自动化构建工具，可以将多个js文件或是css压缩成一个文件，并且可以压缩为一行，
    以此来减少文件体积，加快请求速度和减少请求次数；并且gulp有task定义处理事务，从而构建整体流程，它是基于流的自动化构建工具。

    Webpack是前端构建工具，实现了模块化开发和文件处理。他的思想就是“万物皆为模块”，
    它能够将各个模块进行按需加载，不会导致加载了无用或冗余的代码。所以他还有个名字叫前端模块化打包工具。

    我在实际当中会将两种都选择混合使用。虽然两个都可以进行代码的压缩合并减少代码体积，但gulp.config.js中gulp的代码更加简单易懂，需要压缩合并谁就用哪个方法，而webpack样式合并需要在node环境下下载插件才能使用。另一点，gulp 是基于流的打包工具，需要谁，引用谁，并且他的压缩简单明了，后期维护起来方便，webpack则可以将具体的模块进行划分，需要哪个模块就加载哪个模块，实现按需加载，并且排除掉冗余代码，减少代码体积。

    总结起来就是，gulp是基于流的自动化构建工具，但不包括模块化的功能，如果要用到的话，就需要引入外部文件，
    比如require.js等；而webpack是自动化模块打包工具，本身就具有模块化，并且也具有压缩合并的功能。二者侧重点不同，我认为相互结合使用会提高代码质量和代码的优化

##### 3、RESTFUL APA 设计

###### 什么是 RESTful

REST 与技术无关，代表的是一种软件架构风格，REST 是 Representational State Transfer 的简称，中文翻译为“表征状态转移”
REST 从资源的角度类审视整个网络，它将分布在网络中某个节点的资源通过 URL 进行标识，客户端应用通过 URL 来获取资源的表征，获得这些表征致使这些应用转变状态
所有的数据，不管是通过网络获取的还是操作数据库获得（增删改查）的数据，都是资源，将一切数据视为资源是 REST 区别与其他架构风格的最本质属性
对于 REST 这种面向资源的架构风格，有人提出一种全新的结构理念，即：面向资源架构（ROA：Resource Oriented Architecture）
对互联网上的任意东西都视为资源，他认为一个 url 就是一个资源 比如：http://www.xxx.com/get_user/

###### RESTFUL APA

网络应用程序，分为前端和后端两个部分。当前的发展趋势，就是前端设备层出不穷（手机、平板、桌面电脑、其他专用设备......）。

因此，必须有一种统一的机制，方便不同的前端设备与后端进行通信。这导致 API 构架的流行，甚至出现"API First"的设计思想。RESTful API 是目前比较成熟的一套互联网应用程序的 API 设计理论。

##### 4、sleep 函数的延迟

Sleep 函数可以使计算机程序（进程，任务或线程）进入休眠，使其在一段时间内处于非活动状态。当函数设定的计时器到期，或者接收到信号、程序发生中断都会导致程序继续执行。
sleep()函数的调用需要一个时间作为参数，代表程序执行挂起的时间间隔。通常参数的单位是秒，但在一些更加精确的操作系统中能以毫秒甚至是微秒为单位。 [1]
Windows 系统
在 Windows 操作系统中，sleep()函数需要一个以毫秒为单位的参数代表程序挂起时长，sleep()函数包含在 kernel32.dll 动态链接库中，但在批处理文件中没有可以直接运行的 sleep()函数。可以在如 Windows 2003 资源包等 Windows 工具集中找到 sleep()函数。
Unix 系统
在 Unix 类的操作系统中，调用 sleep()函数需要一个以秒为单位的参数，需要更精确的时间控制可以使用 nanosleep()函数。 [2]
C 语言实例
在 Windows 系统中：

    Sleep(2*1000); //sleep for 2 seconds

在 Unix 系统中：

    sleep(2);   //sleep for 2 seconds

注意编辑
在 VC 中 Sleep 中的第一个英文字符为大写的"S"
在标准 C 中是 sleep（S 不要大写），下面使用大写的来说明，具体用什么看你用什么编译器。简单的说 VC 用 Sleep，别的一律使用 sleep。
Sleep 函数的一般形式:

Sleep(unsigned long);

其中，Sleep()里面的单位，是以毫秒为单位，所以如果想让函数滞留 1 秒的话，应该是 Sleep(1000);
在 Linux 下，sleep 中的“s”不大写

sleep()单位为秒，usleep()里面的单位是微秒。在内核中，sleep 的实现是由 pause 函数和 alarm 函数两个实现的。

特别注意在 Codeblocks 环境下是无法使用 sleep 函数的，因为在 windows 上 Codeblocks 采用 mingw(Gnu 在 Window 环境下的编译器，可以充分使用 WindowsApi)作为编译器，而在 stdlib.h 中 sleep 的说明如下：\_CRTIMP void **cdecl **MINGW_NOTHROW \_sleep (unsigned long) \_\_MINGW_ATTRIB_DEPRECATED;可以认为 mingw 舍弃了 sleep 函数，建议用 Sleep 实现 sleep。

示例编辑

    #include <windows.h>
    #include<stdio.h>
    int main()
    {
    int a;
    a=1000;
    printf("你");
    Sleep(a);/* VC 使用Sleep*/
    printf("好"); /*输出“你”和“好”之间会间隔一千毫秒，即间隔一秒，Sleep()的单位为毫秒*/
    return 0;
    }

##### 5、函数柯里化

其实就是高阶函数的一个特殊用法。

维基百科上说道：柯里化，英语：Currying(果然是满满的英译中的既视感)，是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术

    // 普通的add函数
    function add(x, y) {
        return x + y
    }

    // Currying后
    function curryingAdd(x) {
        return function (y) {
            return x + y
        }
    }

    add(1, 2)           // 3
    curryingAdd(1)(2)   // 3

实际上就是把 add 函数的 x，y 两个参数变成了先用一个函数接收 x 然后返回一个函数去处理 y 参数。现在思路应该就比较清晰了，就是只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

但是问题来了费这么大劲封装一层，到底有什么用处呢？没有好处想让我们程序员多干事情是不可能滴，这辈子都不可能.

来列一列 Currying 有哪些好处呢？

1. 参数复用
   // 正常正则验证字符串 reg.test(txt)


        // 函数封装后
        function check(reg, txt) {
            return reg.test(txt)
        }

        check(/\d+/g, 'test')       //false
        check(/[a-z]+/g, 'test')    //true

        // Currying后
        function curryingCheck(reg) {
            return function(txt) {
                return reg.test(txt)
            }
        }

        var hasNumber = curryingCheck(/\d+/g)
        var hasLetter = curryingCheck(/[a-z]+/g)

        hasNumber('test1')      // true
        hasNumber('testtest')   // false
        hasLetter('21212')      // false

上面的示例是一个正则的校验，正常来说直接调用 check 函数就可以了，但是如果我有很多地方都要校验是否有数字，其实就是需要将第一个参数 reg 进行复用，这样别的地方就能够直接调用 hasNumber，hasLetter 等函数，让参数能够复用，调用起来也更方便。

2.  提前确认

        var on = function(element, event, handler) {
            if (document.addEventListener) {
                if (element && event && handler) {
                    element.addEventListener(event, handler, false);
                }
            } else {
                if (element && event && handler) {
                    element.attachEvent('on' + event, handler);
                }
            }
        }

        var on = (function() {
            if (document.addEventListener) {
                return function(element, event, handler) {
                    if (element && event && handler) {
                        element.addEventListener(event, handler, false);
                    }
                };
            } else {
                return function(element, event, handler) {
                    if (element && event && handler) {
                        element.attachEvent('on' + event, handler);
                    }
                };
            }
        })();

//换一种写法可能比较好理解一点，上面就是把 isSupport 这个参数给先确定下来了

    var on = function(isSupport, element, event, handler) {
        isSupport = isSupport || document.addEventListener;
        if (isSupport) {
            return element.addEventListener(event, handler, false);
        } else {
            return element.attachEvent('on' + event, handler);
        }
    }

我们在做项目的过程中，封装一些 dom 操作可以说再常见不过，上面第一种写法也是比较常见，但是我们看看第二种写法，它相对一第一种写法就是自执行然后返回一个新的函数，这样其实就是提前确定了会走哪一个方法，避免每次都进行判断。

3.  延迟运行

        Function.prototype.bind = function (context) {
            var _this = this
            var args = Array.prototype.slice.call(arguments, 1)

            return function() {
                return _this.apply(context, args)
            }

    }

像我们 js 中经常使用的 bind，实现的机制就是 Currying.

说了这几点好处之后，发现还有个问题，难道每次使用 Currying 都要对底层函数去做修改，

有没有什么通用的封装方法？
// 初步封装

    var currying = function(fn) {
        // args 获取第一个方法内的全部参数
        var args = Array.prototype.slice.call(arguments, 1)
        return function() {
            // 将后面方法里的全部参数和args进行合并
            var newArgs = args.concat(Array.prototype.slice.call(arguments))
            // 把合并后的参数通过apply作为fn的参数并执行
            return fn.apply(this, newArgs)
        }
    }

这边首先是初步封装,通过闭包把初步参数给保存下来，然后通过获取剩下的 arguments 进行拼接，最后执行需要 currying 的函数。

但是好像还有些什么缺陷，这样返回的话其实只能多扩展一个参数，currying(a)(b)(c)这样的话，貌似就不支持了（不支持多参数调用），一般这种情况都会想到使用递归再进行封装一层。

// 支持多参数传递

    function progressCurrying(fn, args) {

        var _this = this
        var len = fn.length;
        var args = args || [];

        return function() {
            var _args = Array.prototype.slice.call(arguments);
            Array.prototype.push.apply(args, _args);

            // 如果参数个数小于最初的fn.length，则递归调用，继续收集参数
            if (_args.length < len) {
                return progressCurrying.call(_this, fn, _args);
            }

            // 参数收集完毕，则执行fn
            return fn.apply(this, _args);
       }
    }

这边其实是在初步的基础上，加上了递归的调用，只要参数个数小于最初的 fn.length，就会继续执行递归。

好处说完了，通用方法也有了，让我们来关注下 curry 的性能
curry 的一些性能问题你只要知道下面四点就差不多了：

存取 arguments 对象通常要比存取命名参数要慢一点
一些老版本的浏览器在 arguments.length 的实现上是相当慢的
使用 fn.apply( … ) 和 fn.call( … )通常比直接调用 fn( … ) 稍微慢点
创建大量嵌套作用域和闭包函数会带来花销，无论是在内存还是速度上
其实在大部分应用中，主要的性能瓶颈是在操作 DOM 节点上，这 js 的性能损耗基本是可以忽略不计的，所以 curry 是可以直接放心的使用。

最后再扩展一道经典面试题
// 实现一个 add 方法，使计算结果能够满足如下预期：

    add(1)(2)(3) = 6;
    add(1, 2, 3)(4) = 10;
    add(1)(2)(3)(4)(5) = 15;

    function add() {
        // 第一次执行时，定义一个数组专门用来存储所有的参数
        var _args = Array.prototype.slice.call(arguments);

        // 在内部声明一个函数，利用闭包的特性保存_args并收集所有的参数值
        var _adder = function() {
            _args.push(...arguments);
            return _adder;
        };

        // 利用toString隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
        _adder.toString = function () {
            return _args.reduce(function (a, b) {
                return a + b;
            });
        }
        return _adder;
        }

        add(1)(2)(3)                // 6
        add(1, 2, 3)(4)             // 10
        add(1)(2)(3)(4)(5)          // 15
        add(2, 6)(1)                // 9

```
    参考的资料
    https://www.zhangxinxu.com/wordpress/2013/02/js-currying/
    https://juejin.im/entry/58b316d78d6d810058678579
    http://blog.jobbole.com/77956/
    https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html
    https://zh.wikipedia.org/zh/%E6%9F%AF%E9%87%8C%E5%8C%96
```

##### 6、Promise 的 retry（重试）功能实现

已有请求函数 getData，其功能为异步请求数据返回 promise 对象，如 getData(params).then(…).catch(…)。实现一个 myGetData,返回 promise 对象，要求加入失败重试功能，该函数内部依然使用 getData 实现，在 getData 失败一次后间隔一秒钟再重试一次，直到重试到第五次、如果全都失败了，myGetData 所返回的 promise 为 reject，只要有任意一次成功，则停止重试，知道 resolve 结果。
假定 getData 的 promise 是一个简单的随机数例子，生成一个随机数大于 10 则 reject，小于 10 则 resolve：

function getData(){
let p = new Promise(function(resolve, reject){
setTimeout(function(){
var num = Math.ceil(Math.random()\*20); //生成 1-10 的随机数
console.log('随机数生成的值：',num)
if(num<=10){
console.log('符合条件，值为'+num)
resolve(num);
}
else{
reject('数字大于 10 了执行失败');
}
}, 2000);
})
return p
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
（2）实现函数 myGetData，也返回一个 promise，但是有失败重试功能：

    function myGetData(getData, times, delay) {
        return new Promise(function(resolve, reject) {
            function attempt () {
            getData().then(resolve).catch(function(erro) {
            console.log(`还有 ${times} 次尝试`)
            if (0 == times) {
            reject(erro)
            } else {
            times--
            setTimeout(attempt(), delay)
            }
            })
            }
        attempt(){
        })
    }

执行结果
我们通过调用 myGetData 来看下执行效果，是否达到五次失败内可以重试：

// 执行函数，五次重试，每隔一秒执行一次
myGetData(getData,5,1000)
