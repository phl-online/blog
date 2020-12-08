# HTML5
## 语义化
* 让页面的内容结构化，便于对浏览器、搜索引擎的解析
* 利于SEO，爬虫可以分析关键词的权衡
* 便于团队的维护和开发
* 方便多设备解析

## 常用的HTML5标签
```html
<!-- 头部 -->
<header>
  <nav></nav>
</header>

<!-- 内容区 -->
<main>
  <!-- 左侧 -->
  <article>
    <!-- 左侧标题 -->
    <header></header>
    <!-- 图片区块 -->
    <figure>
      <img>
      <figcaption></figcaption>
    </figure>
  </article>

  <!-- 右侧 -->
  <aside>
    <!-- 友情链接 -->
    <nav></nav>
    <section></section>
  </aside>
</main>

<!-- 底部 -->
<footer></footer>
```

```html
......
<canvas> // 绘图
<video>
<audio> // 媒介回放
```

```
date、calendar、time // 表单控件
localStorage、sessionStorage// 本地存储
webSocket // 允许服务端主动向客户端推送数据
webworker
...
...
```