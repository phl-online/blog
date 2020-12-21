# 你对this、apply、call、bind的理解？
参考文献
* [x] [「前端料包」一文彻底搞懂JavaScript中的this、call、apply和bind](https://juejin.cn/post/6844904009308831751)

**常见的一些问题汇总**
* [x] [1、apply和call的区别？]()
* [x] [2、什么场景下用apply，什么场景下用call]()


* apply
* **定义：方法能劫持另外一个对象的方法，继承另外一个对象的属性**
* Function.apply(obj,args)
  
  * obj这个对象，将代替Function类里this对象
  * args这个是数组，将作为参数出传给Function(args->arguments)


```js
function Person(name,age) {
  this.name=name;
  this.age=age; 
}
function Student(name,age,grade) {
  Person.apply(this,arguments)
  this.grade=grade;
}
// 创建一个学生类
const name = new Student('张三'，'21','大四');

// nanme:张三 age:21 grade 大四

// 也就说用Student去执行Person这个类里面的内容，Person这个类里面的存在this.name等之类语句，也就是等同于在Student对象里面也创建了同样属性
```


* call
* 字面意思和apply相同
* Function.apply(obj,[param1[,param2[,...[paramN]]]])

  * obj这个对象，将代替Function类里this对象
  * params，这个是一个参数列表

```js
// 可以直接将上面的Student的函数里面apply换成call
Person.call(this,name,age)
```


