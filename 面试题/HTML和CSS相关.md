## 1、对语义化标签的理解

语义化就是根据不同的结构选择合适的标签，简单来说的就是使用正确的标签做正确的事情

比如：头部标签`<header></header>`、导航栏`<nav></nav>`、区块`<section><section>`、底部`<footer>` 等等

语义化的优点有很多：

1、方便维护，增强了可读性，页面结构一目了然；

2、对机器友好，有利于SEO和搜索引擎的爬虫抓取更多的有效信息，因为爬虫依赖于标签来确定上下文和各个关键字的权重；

3、在去掉或者丢失样式的时候能够让页面也能呈现出清晰的结构；

4、方便其他设备解析，比如屏幕阅读器、盲人阅读器、移动设备等，可以根据语义渲染页面，或者自动生成文章目录；


## 2、渐进增强和优雅降级

1、渐进增强

针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验

2、优雅降级

一开始就构建完整的功能，然后再针对低版本浏览器进行兼容


## 3、适配相关

1、px + viewport适配

2、rem布局

    CSS3媒体查询适配

    基于设计图的rem布局

    基于屏幕百分比的rem布局

    小程序的rpx布局

3、vw布局

    通过媒体查询的方式即 CSS3 的 meida、queries

    以天猫首页为代表的 flex弹性布局

    以淘宝首页为代表的 rem+viewport 缩放

    rem方式


## 4、响应式布局 

响应式页面：是指一套页面，能够适配多种终端，比如宽屏pc电脑、平板、手机

1、响应式布局的三个CSS技术

    百分比流式布局（Liquid Layout）

    网格系统 (Grid System)

    @media 媒体查询

2、流式布局（Liquid Layout）就是百分比布局，也称非固定像素布局

    将页面的宽度设置成百分比，根据窗口的宽度来进行伸缩，不受固定像素的限制

    使用%百分比定义宽度（搭配min-*、max-*属性使用），高度大都是用px来固定住，可以根据可视区域 (viewport) 和父元素的实时尺寸进行调整，尽可能的适应各种分辨率。

3、栅格系统 (Grid System)

    Grid布局是一种新的 CSS 布局模型，比较擅长将一个页面划分为几个主要区域，以及定义这些区域的大小、位置、层次等关系。它使用一系列容器、行和列来布局和对齐内容，这就意味着它可以同时处理列和行 媒体查询@media

    媒体查询@media由媒体类型和一个或多个检测媒体特性的条件表达式组成。
    
    通过媒体查询@media提供的一系列条件，我们可以对页面进行不同的布局组合，当媒体查询结果满足某些条件时，布局上进行相应的变化。
    
    例如，当分辨率小于多少时，窗口由320变为750，需要调整对应的样式，一般和rem或em组合使用


## 5、HTML5 新特性 

1、拖拽释放（drag and drop）API

2、语义化更好的内容标签（header footer nav aside article section）

3、音频、视频（audio video）API

4、画布（Canvas）API

5、地理（Geolocation）API

6、localstorage 和 sessionstorage 缓存方式

7、表单控件（calendar date time email ul search）


## 6、localstorage 和 sessionstorage 

localStorage生命周期是永久，这意味着除非用户显示在浏览器提供的UI上清除localStorage信息，否则这些信息将永远存在；

sessionStorage生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过sessionStorage存储的数据也就被清空了；

localStorage支持5M大小的存储，并且一次请求就能存储在localStorage；

以后的每次调用都是直接从localStorage读取数据，不必再此请求接口，节省时间。


## 7、transform 有哪些值

Transform属性应用于元素的2D或3D转换。这个属性允许你将元素旋转，缩放，移动，倾斜等；

有rotate() 含义：旋转；deg是“度”的意思、skew() 含义：倾斜；

scale() 含义：比例；“1.5”表示以1.5的比例放大，缩小则为负-
translate(,)含义：位移的意思；

分别还有x、y之分，比如：translatex() 和 translatey() ，以此类推。

transform属性有很多这是经常使用的一些属性


## 8、使元素消失的方法有哪些

常见的有以下几种：

1、设置display:none;在网页中不占任何的位置

2、设置visibility:hidden;将元素隐藏，但是在网页中该占的位置还是占着

3、设置opacity:0; 让元素透明

4、设置position:absolute;给top或left一个负值让元素移除页面显示范围

5、设置height:0;


## 9、什么是 BFC

BFC（Block Formatting Context）块格式化上下文， BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素

产生bfc的条件：

1、浮动元素，float 除 none 以外的值；

2、定位元素，position（absolute，fixed）；

3、display 为以下其中之一的值 inline-block，table-cell，table-caption；

4、overflow 除了 visible 以外的值（hidden，auto，scroll）；

bfc特性：

1、内部的Box会在垂直方向上一个接一个的放置。

2、垂直方向上的距离由margin决定

3、bfc的区域不会与float的元素区域重叠。

4、计算bfc的高度时，浮动元素也参与计算

5、bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。

bfc的作用：

1、解决外边距折叠,上下两个盒子，第一个div的margin-bottom设置为10px，第二个div的margin-top设置为20px，

2、可以看到两个盒子最终的边距是20px，这就是外边距重叠的问题，解决这个问题，
为 第一个盒子套一个父盒子，这样两个盒子属于不同的bfc，该问题解决

以上就是我对bfc的认识。


## 10、@import 和 link 引入样式的区别

link 引用 css 时，在页面载入时同时加载；而 @import 需要页面网页完全载入以后加载，可能会出现闪屏

link 是 XHTML 标签，无兼容问题，除了能加载CSS外还能加载RSS；而 @import 是在 CSS2.1 时提出的，低版本的浏览器不支持，只能加载 CSS

link 支持使用 JavaScript 控制 DOM 去改变样式；而 @import 不支持


## 11、position 的理解 

Position是用来给元素进行定位的，常用属性有4个：

static是position的默认属性，元素会按正常的文档流进行排序，不会受到top，bottom，left，right的影响

relative相对于元素正常位置进行偏移，受top，bottom，left，right的影响。多用于absolute绝对定位的父元素。

absolute绝对定位会脱离文档流，相对于最近的定位（非static）父级元素定位，若父级元素没有定位，则相对于浏览器窗口定位。

fixed固定定位脱离文档流，一直相对于浏览器窗口定位，无论页面如何滚动，此元素总在屏幕的同一个位置。

注：当fixed定位的父元素使用了transform的样式时，fixed定位会失效，变成和absolute一样的效果。

使用：常与z-index和display js联合式用，实现网页中的效果，如图片轮播，吸顶效果等


## 12、边界塌陷（高度坍塌问题）

1、边界塌陷：

同时 给兄弟/父子设置上下边距，理论上是两者之和 ，事实上只有一半，这种现象 是边界塌陷

注： 浮动和定位不会产生边界塌陷，只有块级元素 垂直方向上才会产生 margin合并

解决方案：

    margin 同为正数 或者 负数 取的绝对值大的数、margin 一正 一负 求两者的和

2、父子元素： 边界塌陷：
            
    父元素：调整内边距、overflow:hidden ---> 出发bfc 看作是一个隔离的元素，不会影响其他元素

    子元素：display:inline-block、overflow:hidden、浮动 和 定位


## 13、src与href的区别 

src 和 href 都可以加载外部文件

区别：

1、href 用在 link 和 a 上，src 用在 js 、img 等标签上

2、当浏览器遇到 href 会并行下载资源并且不会停止对当前文件的处理

3、当浏览器解析到 src 会暂停其他资源的下载和处理，直到将该资源加载或执行完毕


