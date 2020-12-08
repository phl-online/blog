#  BFC
> BFC：块级格式化上下文 Block Formatting Context<br/>
> 首先就需要知道 Box Formatting Context 分别指的是什么


## Box：CSS布局的基本单位
常见盒子：<br/>
* block-level box：display 属性为block，
* inline-level box：dispaly 属性为inline
* run-in box：css3 特有
  

## Formatting Context
* 指的是页面的一块渲染区域
* 有自己的渲染规则，决定了其元素如何定位，以及和其他元素的关系和相互作用

## BFC解决问题
* 清除浮动
* 避免margin重叠


## 产生BFC的条件
* 根元素html
* 浮动元素 float/left/right
* 绝对定位 position/absolute/fixed
* 内联块 display/inline-block/flex/grid
* overflow:hidden