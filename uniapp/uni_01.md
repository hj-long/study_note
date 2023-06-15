## Uniapp

适合中小公司、一套代码可以多端发布、节省人力、时间

但是，不同端的代码个性化展示需要进行条件编译


## 目录结构

* pages             页面

* components        组件

* static            静态资源(图片、css文件)

* App.vue           整个uniapp项目的第一个组件

* index.html        入口html

* main.js           第一个运行的js文件(全局js文件)(和vue-cli的main.js差不多)

* manifest.json     全局文件 -->  应用的配置

* pages.json        全局文件 -->  页面的配置

* uni.scss          全局样式文件


## 什么是条件编译？

在不同端，展示不同代码



## 面试题

- 问题一：如果某一段代码想在某一端展示该怎么办？

    答：条件编译，




## 结构

### 一、template: 布局盒子（视图）

1. view： 类似于 div ,块级容器

2. text：  放入文字的（可以添加属性让它长按选中等特性），行内元素

3. 表单组件:  button、form、input、label


### 二、CSS

1. 尺寸单位： rpx --> 响应式px

    ui的设计图如果宽度是750, 但是测量出来的元素宽度是300px,那么就在uniapp中的style部分写300rpx就可以了

### 三、js

1. 语法跟 vue 基本一样

2. 生命周期：分为 页面生命周期、组件生命周期

#### 页面生命周期

- uniapp 中的页面：指的是 pages 目录下的 .vue 文件

    - 流程：先在 pages 目录下新建 .vue 或者 .nvue 文件，并且在 pages.json 中进行配置即可

    - pages目录下.vue文件 拥有页面生命周期，同时 也拥有 组件生命周期

    - onLoad：第一次进入页面所执行的生命周期
            
        - 会接收 页面传递过来的 参数

        - 请求接口

    - onShow：  每一次进入页面所执行的生命周期

    - onShow：  每一次进入页面所执行的生命周期

    - onReady   页面渲染完成

            场景:可以获取dom

    - onHide    页面隐藏时触发

    - onUnload  页面卸载时触发

#### 组件生命周期

- uniapp 中的组件：不是在pages目录下新建的.vue文件

    - 如果在组件vue文件中写了 页面的生命周期，则不会生效

    - 组件生命周期：

        ```
        beforeCreate
        created
        beforeMount
        mounted
        beforeUpdate
        updated
        beforeDestroy
        destroyed
        ```


#### 路由页面跳转

1. api方法

    - uni.navigateTo(OBJECT)    保留当前页面，跳转到应用内的某个页面

    - uni.redirectTo(OBJECT)    关闭当前页面，跳转到应用内的某个页面

    - uni.reLaunch(OBJECT)      关闭所有页面，打开到应用内的某个页面

    - uni.switchTab(OBJECT)     跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面

    - uni.navigateBack(OBJECT)  关闭当前页面，返回上一页面或多级页面。可通过 getCurrentPages() 获取当前的页面栈，决定需要返回几层。
    

注意: 如果跳转到的是tabbar页面请用``switchTab``否则请用 ``navigateTo、redirectTo...``


2. 组件

    - navigator 标签






### 小程序

微信小程序

百度小程序

支付宝小程序

。。。。

1. 需要兼容哪一款小程序，就要去下载那个小程序的开发者工具

2. 去运行


### web


### app

1. ios

    不再支持基座调试 => 模拟器: xCode

2. 安卓

    安装 Android Stdio