# Vue中组件传值方法有哪些？
参考文献
  * [x] [【Vue】组件传值的六种方法](https://www.jianshu.com/p/1ecba3a93f12)

## 父子组件通信
```js
 props //父->子
 $emit、$on //子->父
 ref // 获取实例方式调用组件的属性或者方法 //子->父
 $parent / $children  //获取父子组件实例
```
* 一些实际🌰

  * **子组件向父组件传值   ref/refs**<br/>
ref 如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；<br/>**如果用在子组件上，引用就指向组件实例，可以通过实例直接调用组件的方法或访问数据。也算是子组件向父组件传值的一种**。

  父组件
  ```js
  <template>
    <child ref="childForRef"></child>
  </template>
  <script>
  import child from './child.vue'
    export default {
      components: { child },
      mounted() {
        const childForRef = this.$refs.childForRef;
        console.log(childForRef.name);
        childForRef.sayHello();
      }
    }
  </script>
  ```
  子组件
  ```js
  export default {
    data () {
      return {
        name: 'Vue.js'
      }
    },
    methods: {
      sayHello () {
        console.log('hello')
      }
    }
  }
  ```





## 兄弟组件通信
```js
eventBus  Vue.prototype.$bus = new Vue
vuex
```

## 跨级通信
```js
vuex
$ attrs / $ listeners   
// $ attrs 里面存放的父组件中绑定的非Props属性
// $ listeners 里面存放的父组件绑定的非原生事件
provide / inject 
// provide:一个对象或返回一个对象的函数 
// inject:一个字符串数组，或 一个对象，对象的 [key] 是本地的绑定名  
```

* 一些实际🌰
  * **兄弟组件和隔代组件**   eventBus
  