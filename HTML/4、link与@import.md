# link与@import
link、@import，都是用来引入css

## 两种方式的区别
* 加载顺序
  * link引用的css会被同时加载，
  * @import引用的css会等页面全部被下载完再加载
* 兼容性区别
  * link 全部都兼容
  * @import 至少的IE9以上 
* 引入的内容不同
  * link 还可引入图片等资源文件
  * @import 只引用样式文件
* 对JS的支持不同
  * link 支持使用js去控制DOM去改变样式
  * @import不支持 