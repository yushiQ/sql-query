##### proxy 和 Reflect 设置跨域
###### proxy
 Proxy(代理)是 ES6 中新增的一个特性。Proxy 让我们能够以简洁易懂的方式控制外部对对象的访问。其功能非常类似于设计模式中的代理模式。

使用 Proxy 的好处是：对象只需关注于核心逻辑，一些非核心的逻辑（如：读取或设置对象的某些属性前记录日志；设置对象的某些属性值前，需要验证；某些属性的访问控制等）可以让 Proxy 来做。从而达到关注点分离，降级对象复杂度的目的。
https://www.jianshu.com/p/34f0e6abe312

使用语法：
var p = new Proxy(target, handler);
其中，target 为被代理对象。handler 是一个对象，其声明了代理 target 的一些操作。p 是代理后的对象。
当外界每次对 p 进行操作时，就会执行 handler 对象上的一些方法。

    handler 能代理的一些常用的方法如下：
    1、get(target,key)：读取
    2、set(target,key,value)：修改
    3、has(target,key)：判断对象是否有该属性
    4、deleteProperty(target,key)：删除某个属性
    5、ownKeys(target)：获取所有的属性名
    6、construct：构造函数  

###### Reflect
Reflect（反射）是ES6为更方便的操作对象而提供的新API 。

这个API设计的目的只要有：

1.将Object上明显属于语言内部的方法放到Reflect上

2.修改某些Object上方法的返回结果，使其变得合理

3.让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。

4.Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。
这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

    Reflect对象一共有13个静态方法：
    Reflect.apply(target,thisArg,args)
    Reflect.construct(target,args)
    Reflect.get(target,key,receiver)
    Reflect.set(target,key,value,receiver)
    Reflect.defineProperty(target,key,desc)
    Reflect.deleteProperty(target,key)
    Reflect.has(target,key)
    Reflect.ownKeys(target)
    Reflect.isExtensible(target)
    Reflect.preventExtensions(target)
    Reflect.getOwnPropertyDescriptor(target, key)
    Reflect.getPrototypeOf(target)
    Reflect.setPrototypeOf(target, prototype)
```
Proxy和Reflect的实际使用
对设置数据进行规则验证：

  // 定义一个校验函数，返回代理对象
  function validator(target,validator){
    return new Proxy(target,{
      _validator:validator,
      set(target,key,value,proxy){
        if(target.hasOwnProperty(key)){
          let va=this._validator[key];
          // 有这个属性名并且设置的值符合校验规则才能设置
          if(!!va(value)){
            return Reflect.set(target,key,value,proxy)
          }else{
            throw Error(`不能设置${key}为${value}`)
          }
        }else{
          throw Error(`${key} 不存在`)
        }
      }
    })
  }
  // 校验规则
  let personValidators={
    name(val){
      return typeof val==='string'
    },
    age(val){
      return typeof val === 'number' && val > 18
    }
  }

  class Person{
    constructor(name,age){
      this.name=name;
      this.age=age;
            // 传入this类的实例，校验规则
      return validator(this,personValidators)
    }
  }
  //实例化类Person
  let person=new Person('xf',30);

  console.info(person);

  // person.name=22; //报错，不让设置
  person.name='winne';

  console.log(person);
```
