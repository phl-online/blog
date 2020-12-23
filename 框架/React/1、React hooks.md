# React hooks
**参考文献**
* [x] [烤透 React Hook](https://juejin.cn/post/6867745889184972814)
* [x] [useEffect 完整指南](https://overreacted.io/zh-hans/a-complete-guide-to-useeffect/)
* [x] [React With Reudx Hooks详解](https://juejin.im/post/6888529255244759047#heading-16)

<br/>

| react-hooks api |
| --- |
| [useState](#chapter-one) |
| <a name="catalog-chapter-two" id="catalog-chapter-two"></a>[useEffect](#chapter-two) |
| <a name="catalog-chapter-three" id="catalog-chapter-three"></a>[useContext](#chapter-three) |
| <a name="catalog-chapter-four" id="catalog-chapter-four"></a>[useCallback](#chapter-four) |
| <a name="catalog-chapter-five" id="catalog-chapter-five"></a>[useRef](#chapter-five) |
| <a name="catalog-chapter-six" id="catalog-chapter-six"></a>[useMemo](#chapter-six) |
<!-- 目录结束 -->
<br/>

## 具体使用
* <a name="chapter-one" id="chapter-one"></a>**1、useState**
* **1.1、基本语法**
  
  ```js
  const [state,setState]=useState(initialState)
  ```

  * 传入的唯一参数：`initialState`，可以是
    * `数字 useState(0)`
    * `字符串`
    * `对象 useState({a:1})`
    * `数组 useState[1,2]`
    * `可以传入函数，来通过逻辑计算出默认值`
    ```js
    function App (props) {
      const [ count, setCount ] = useState(() => {
        return props.count || 0
      })
      return (
        <div>
          点击次数: { count } 
          <button onClick={() => { setCount(count + 1)}}>点我</button>
        </div>
      )
    }
    ```
  * 返回的是包含`两个元素`的数组
    * `第一个state 变量`
    * `第二个 setState 是修改state值的方法，是一个函数`
  
  


* **1.2、看个**🌰
  ```js
  import React, { useState} from "react";
  const [count, setCount]= useState(0)

  return(
    <div>
      点击次数：{count}
      <button  onClick={() => { setCount(count + 1)}}>点我</button>
    </div>
  )
  ```
  上面例子有些需要注意的地方

  * 传入相同值，是不会重新渲染的
  * 组件每渲染一次，useState中函数其实不会每次都执行
  * setUseState时获取上一轮值
  ```js
  setCount(count=>count+1)
  ```
  * 多个useState的情况，尽量不要在循环，条件或嵌套函数中调用hook，必须确保是在React函数的最顶层调用。确保hook在每一次渲染中都按照同样的顺序被调用。


* <a name="chapter-two" id="chapter-two"></a>**2、useEffect**
* **2.1、基本描述**
  
  * 主要提供了页面钩子方法，完成一些类似于class组件中生命周期<br/>`componentDidMount` 挂载<br/> `componentDidUpdate` 更新<br/>`componentWillUnmount` 卸载<br/>的功能
  * 例如像：网络请求、手动更新`DOM`、一些事件的监听，都是`React`更新`DOM`的一些副作用
  * 举个🌰
  ```js
  import React, { useEffect} from "react";
  useEffect(()=>{
    console.log('useEffect被执行了')
  })
  ```
* **2.2、基本用法**
* 2.2.1、作用：通过`useEffect`的Hook，告诉`React`需要在渲染后执行某些操作
* 2.2.2、参数：`useEffect`要求我们传入一个回调函数，在`React`执行完更新`DOM`操作之后，就会回调这个函数
* 2.2.3、执行时机：首次渲染之后，或者每次更新状态之后，都会执行这个回调函数
 
  引起注意的是，useEffect的第二个参数
  * 什么都不传，组件每次 render 之后 useEffect 都会调用 <===> 等同于：`componentDidMount` 和 `componentDidUpdate`
   ```js
    useEffect(()=>{
      //TODO
      //具体逻辑
    })
    ```
    * 传入一个空数组，只会调用一次， <===> 等同于：`componentDidMount` 和 `componentDidUpdate`
    ```js
    useEffect(()=>{
      //TODO
      //具体逻辑
    },[])
    ```
    * 传入一个数组，其中包括变量，只有这些变量变动时，`useEffect`才会执行
    ```js
    useEffect(()=>{
      //TODO
      //具体逻辑
    },[count])
    ```

* 🤔 **2.3、有些时候需要清除Effect**
* 2.3.1、最常见的场景：清除订阅外部数据源，防止引起内存泄露<br/>

  参考代码如下
  ```js
  import React, { useEffect, useState } from 'react'
  useEffect(() => {
    // 默认情况下，每次渲染后都会被调用该函数
    console.log('订阅一些事件')
    
    // 如果要实现 componentWillUnmount，
    // 在末尾处返回一个函数
    // React 在该函数组件卸载前调用该方法
    // 其命名为cleanup 是为了表明此函数的目的
    // 但其实也可返回一个箭头函数或者起一个别名
    return function cleanup() => {
      console.log('取消订阅')  //取消订阅
    }
  })
  ```

- 2.3.2、为啥要在`Effect`中返回一个函数？
	
    - 这是因为`Effect`可选的清除机制，每个`Effect`都可返回一个清除函数
	- 如此可以将添加和移除订阅的逻辑放到一起
    
	- 都是属于`Effect`的一部分
  
* <a name="chapter-three" id="chapter-three"></a>**3、useContext**
























* <a name="chapter-four" id="chapter-four"></a>**4、useCallback**

* **4.1、作用**
  
  * 尤其向子组件传递函数props时，每次render，都会创建新函数，导致子组件不必要的渲染，浪费性能
  ```js
  const onClick = useCallback(()=>{
    console.log('button click')
  },[])
  ```
  * 解决当依赖的属性没有改变时

* **4.2、基本使用**
  
  *  `useCallback`主要是为了进行性能优化
  *  










* <a name="chapter-five" id="chapter-five"></a>**5、useRef**
* **5.1、基本用法**
  
  * `useRef`返回一个`ref`对象，返回的`ref`对象在组件的整个生命周期保持不变
  ```js
  const refContainer=useRef(initialState)

  const refContainer=useRef(0)
  // 返回结果
  {current: 0}
  ```
* **5.2、常见的使用场景**
  
  * 场景一：保存一个数据，这个对象在整个生命周期可以保持不变
  ```js
  const [count, setCount] = useState(0);
  const numRef = useRef(count);

  // 将上次的count进行保存，在count发生改变时,重新保存count
  // 在点击button时,增加count时,会调用useEffect函数,渲染DOM后,会重新将上一次的值进行保存,使用ref保存上一次的某一个值不会触发render
  useEffect(() => {
    numRef.current = count;
  }, [count]);

  <div>count 上次的值：{numRef.current}</div>
  <div>count 这次的值：{count}</div>
  <button onClick={(e) => setCount(count + 10)}>+10</button>
  ```

  * 场景二：引入`DOM`(或者组件)元素，但是前提条件需要是`class`组件
  
  ```js
  class childTest extends React.Component {
    render() {
      return <div>childTest</div>
    }
  }

  export default function demo() {
    const titleRef = useRef()
    const cpnRef = useRef()

    function changeDOM() {
      // 修改DOM
      titleRef.current.innerHTML = 'hello world'
      console.log(cpnRef.current)
    }

    return (
      <div>
        {/* 1.修改DOM元素 */}
        <h2 ref={titleRef}>RefHookDemo01</h2>
        {/* 2.获取class组件 */}
        <ChildCpn ref={cpnRef} />
        <button onClick={changeDOM}>修改DOM</button>
      </div>
    )
  }
  ```




* <a name="chapter-six" id="chapter-six"></a>**6、useMemo**

* **6.1、作用：**
  
  * 6.1.1、为了进行性能的优化
  * 6.1.2、和Vue中的`computed`计算属性类似，都是根据依赖的值计算出结果，如果依赖的值未发生改变的时候，不触发状态改变
  ```js
  const [count, setCount] = useState(0);
  const add = useMemo(() => {
    return count + 1;
  }, [count]);

  <div>
    点击次数: {count}
    <br />
    次数加一: {add}
    <button
      onClick={() => {
        setCount(count + 1);
      }}
    >
      点我
    </button>
  </div>
  ```

* **6.2、注意的是：**
  
  * 6.2.1、 `useMemo`会在**渲染的时候执行**，而不是渲染之后执行，这个与`useEffect`，不一样
  * 6.2.2、`useMemo`，也还是会有第二个参数，用法和`useEffect`类似  