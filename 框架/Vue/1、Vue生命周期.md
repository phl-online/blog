# Vue 生命周期
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64a86f9e3ba5420ab0ecc01b6eb034a8~tplv-k3u1fbpfcp-watermark.image)

- **beforeCreate**，创建前状态，（当前阶段data,methods,computed,watch上的数据和方法都不能被访问）
- **created**，进入数据观测阶段，可以使用和更改数据，（但是不会触发updated函数，当前阶段无法与Dom进行交互）
- **beforeMount**，发生在挂载前
- **mounted**，挂载完成后，（当前的真实的Dom挂载完毕,数据完成双向绑定，可以访问到Dom节点，使用$ refs 属性对Dom 进行操作）
- **beforeUpdate**，发生在更新前，可以在当前阶段进行更改数据，不会造成重复渲染
- **updated** ，发生在更新后，当前阶段Dom已完成更新。
- **beforeDestroy**，销毁前，当前阶段实例完成可以被使用，此阶段可以善后处理一些东西，例如定时器
- **destroyed**，销毁后，此时已经Dom已之剩下空壳，组件已被拆解，数绑定被卸除，监听被移除，

