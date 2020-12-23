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
* `一些实际`🌰

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
* `一些实际`🌰
* **兄弟组件和隔代组件**   eventBus
    eventBus 又称为事件总线，在Vue中可以使用它作为沟通的桥梁，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件， 所以组件都可以通知其他组件。

* 首先需要创建一个事件总线并将其导出, 以便其他模块可以使用或者监听它。
    bus.js:
    ```js
    import Vue from 'vue'
    export const bus = new Vue()
    ```

* 发送事件，假设有 child1、child2 两个兄弟组件，在 child1.vue 中发送事件。
    parent.vue:
    ```js
    <template>
      <div>
        <child1></child1>
        <child2></child2>
      </div>
    </template>
    <script>
    import child1 from './child1.vue'
    import child2 from './child2.vue'
    export default {
      components: { child1, child2 }
    }
    </script>
    ```

  child1.vue:
  ```js
  <template>
    <div>
      <button @click="additionHandle">+加法器</button>    
    </div>
  </template>
  <script>
  import {bus} from '@/bus.js'
  console.log(bus)
  export default {
    data(){
      return{
        num:1
      }
    },
    methods:{
      additionHandle(){
        bus.$emit('addition', {
          num:this.num++
        })
      }
    }
  }
  </script>
  ```

  在 child2.vue 中接收事件。

  ```js
  <template>
    <div>计算和: {{count}}</div>
  </template>
  <script>
  import { bus } from '@/bus.js'
  export default {
    data() {
      return {
        count: 0
      }
    },
    mounted() {
      bus.$on('addition', arg=> {
        this.count = this.count + arg.num;
      })
    }
  }
  </script>
  ```

  如果想移除事件的监听, 可以像下面这样操作：
  ```js
  import { bus } from './bus.js'
  Bus.$off('addition', {})
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

* `一些实际`🌰  
* 兄弟组件和隔代组件   Vuex

  vuex的 store.js
  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  Vue.use(Vuex)
  const state = {
     // 初始化A和B组件的数据，等待获取
     AMsg: '',
     BMsg: ''
   }

   const mutations = {
     receiveAMsg(state, payload) {
       // 将A组件的数据存放于state
       state.AMsg = payload.AMsg
     },
     receiveBMsg(state, payload) {
       // 将B组件的数据存放于state
       state.BMsg = payload.BMsg
     }
   }

   export default new Vuex.Store({
     state,
     mutations
   })
  ```
* 兄弟组件和隔代组件   $attrs / $listeners
  parent.vue
  ```js
  <template>
    <div>
      <child-a :name="name" :age="age" :gender="gender" :height="height" title="嘿嘿嘿"></child-a>
    </div>
  </template>
  <script>
  import ChildA from './ChildA'
  export default {
    components: { ChildA },
    data() {
      return {
        name: "zhang",
        age: "18",
        gender: "女",
        height: "168"
      };
    }
  };
  </script>
  ```
  // childCom1.vue
  ```js
  <template class="border">
    <div>
      <p>name: {{ name}}</p>
      <p>childCom1的$attrs: {{ $attrs }}</p>
      <child-com2 v-bind="$attrs"></child-com2>
    </div>
  </template>
  <script>
  const childCom2 = () => import("./childCom2.vue");
  export default {
    components: {
      childCom2
    },
    inheritAttrs: false, // 可以关闭自动挂载到组件根元素上的没有在props声明的属性
    props: {
      name: String // name作为props属性绑定
    },
    created() {
      console.log(this.$attrs);
       // { "age": "18", "gender": "女", "height": "158", "title": "程序员成长指北" }
    }
  };
  </script>
  ```
  // childCom2.vue
  ```js
  <template>
    <div class="border">
      <p>age: {{ age}}</p>
      <p>childCom2: {{ $attrs }}</p>
    </div>
  </template>
  <script>

  export default {
    inheritAttrs: false,
    props: {
      age: String
    },
    created() {
      console.log(this.$attrs); 
      // { "gender": "女", "height": "158", "title": "程序员成长指北" }
    }
  };
  </script>
  ```
* 兄弟组件和隔代组件   provide / inject
  代码执行顺序<br/>
  data<br/>
  provide<br/>
  created<br/>
  mounted<br/>