# vue2
## 一、介绍
1. vue 就是一个 js 文件: vue.js
2. 官方叫法： 渐进式 javaScript 框架
3. 为什么要设计成一个 渐进式的框架？
    * vue.js 是一个核心库（相当于毛坯房）
    * 可以根据自己需要添加各种功能插件（安装下载）：都是依赖于vue.js
    * 例如：vue.js + router.js + vuex.js + elementui + xx.js

也就是从 毛坯房 + 家具 + ...... 渐进式地搭建和完善项目

## 二、脚手架（vue-cli）

### 2.1 介绍

1. 点击 [官网](https://cli.vuejs.org/zh/) 进行下载安装等工作
    
2. 测试 ``vue -v`` 有相关提示即表示安装成功

3. 创建项目： ``vue create project_name`` 然后根据自己的需要选择创建项目

4. 启动项目： 

    ```
    cd project_name
    npm run serve
    ```

### 2.2 脚手架目录结构

1. vue.config.js： 配置文件
    
    * 可以用来配置脚手架的一些功能，具体看 [官方文档](https://cli.vuejs.org/zh/config/#vue-config-js)


2. README.md: 项目说明文件

    * 项目的介绍说明文档


3. package.json: 项目的模块依赖和一些项目配置

    * name、version: 项目名称、版本

    * scripts： 调试命令，将键名serve改成 aaa，然后就可以用 ``npm run aaa`` 实现 ``npm run serve`` 的效果，一样的，只是改了名称。


4.  node_modules目录: npm下载的模块包存放地

    * ``npm install xx`` 默认下载到当前项目的node_modules文件夹中，属于局部安装，跟跟--save一样
    
    * ``npm install xx -s`` 其中 s 表示 --save，生产环境依赖，在项目中构建项目，是项目的一部分

    * ``npm install xx -d`` 其中的 d 表示 --save-dev，开发环境依赖，用于打包、解析代码的，项目上线时并不需要使用到的

    * ``npm install xx -g`` 加了 -g 就会放到系统的 node_modules文件夹，成为全局模块。


5. src 目录：

    \- main.js: 全局js文件

    \- App.vue: 第一个入口的 vue 文件

    \- assets: 存放静态资源，如图片

    \- components: 存放组件文件


6. public 目录：

    \- favicon.ico: 网页图标

    \- index.html： 项目运行时 http://localhost:8080/ 访问的页面文件，主要是 ``<div id="app"></div>`` 这个dom，通过webpack等一系列工具打包编译的js文件会自动加到这个html文件中，从而使vue项目运行起来

    ``http://localhost:8080/`` 访问的是： ``http://localhost:8080/index.html``，也就是我们vue项目的 public 目录，因此我们可以在浏览器输入 ``http://localhost:8080/favicon.ico`` 访问到网页图标，但是一般不会这样使用，静态资源一般放服务器上，了解就好


## 三、Vue 的开发模式

1. 入口： public ==> index.html(main.js) ==> app.vue

2. 旧的开发方式：一个页面有 html\js\css 等文件组成，而操作由js的getElementById等方法获取dom进行一系列的操作

3. Vue的开发方式：数据驱动模式（数据驱动dom）

    * 页面布局、js、css 都在 .vue文件去写


### .vue 文件组成（三个部分）

1. template标签： 写dom盒子，在vue2中，template标签下只能存在一个父元素标签

2. script标签： 写代码逻辑

3. style标签： 写样式


### vue基本使用

1. 在 data函数 中定义数据：

```
<script>
export default {
    data() {
        return {
            data1: '第一个数据'
        }
    }
}
</script>
```

2. 在 template 中使用数据

```
<template>
  <div id="app">
    <h1>{{ data1 }}</h1>
  </div>
</template>
```

3. 在 style 中定义布局样式

```
<style>
#app {
  text-align: center;
  color: #2c3e50;
}
</style>
```

## 四、vue 部分指令

1. v-for: 循环指令，一般配合 key 来使用，key值必须唯一

    ```
    v-for= "item in data" :key='item'
    v-for= "(item, index) in data" :key='index'

    ##其中，data可以是数组，也可以是对象。
    ```

2. v-if、v-else-if 、v-else: 判断指令，为真则进行创建渲染，反之则删除

    ```
        v-if="false"  条件假：直接把元素删除了，或者是没有创建这个元素

        v-show="true" 条件真：重新创建一个元素，并渲染页面上面来

    ```


3. v-show: 显示和隐藏指令，原理是使用css的 display:none; 进行控制隐藏

    ```
        v-show="false" 还存在页面上，但是是一个隐藏元素（标签style属性添加了display:none;）

        v-show="true" 是一个正常显示元素（正常标签，没有添加style）

    ```

**面试题：** v-show 和 v-if 的区别

1. v-show 和 v-if 条件都是 false 的时候

    * v-show 是隐藏（显示和隐藏元素）

    * v-if 是删除 （v-if是创建和删除盒子）

2. 在首次加载页面（开销问题）时

    * v-show ==> 就算设置了 false，元素也是存在的，只不过隐藏了

    * v-if ==> false时是不存在的，当设置为 true 时才创建元素出来

3. 当频繁的切换时

    * v-show css控制显示和隐藏，开销比较小

    * v-if 创建和删除，不断地创建和删除元素，开销会大很多


### 五、vue 定义方法

1. 在 methods 中定义方法func， 在标签中使用 @事件名='func' 进行事件的绑定

    在 data函数 中定义数据，在 methods对象 中定义方法，可以使用 this.xx 操作 data 中 xx 数据

    ```
    <template>
    <div class="hello">
        <h1>{{ data1 }}</h1>
        <button @click="func">点击</button>
    </div>
    </template>

    <script>
    export default{
        data() {
            return {
                data1: 1 
            }
        },
        methods: {
            func() {
                this.data1 += 1
                console.log('我是一个方法')
            }
        }
    }
    </script>

    ```


## 六、vue 数据流

1. data 函数返回的对象中的属性会成为当前组件的 Vue 实例的响应式数据：
    * 当 Vue 实例创建时，这些data中的属性会被添加到 Vue 实例上作为其属性，并且会在组件的生命周期中被使用

    * 生命周期：
        ```
        1. beforeCreate: 组件实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用

        2. created: 组件示例创建完成后立即调用

        3. beforeMount: 在挂载开始之前被调用，相关的 render 函数首次被调用。

        4. mounted: el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子函数。

        5. beforeUpdate: 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前

        6. updated: 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子函数

        7. beforeDestroy: 实例销毁之前调用，在这一步，实例仍然完全可用

        8. destroy: 实例销毁后调用。此时，所有的事件监听器已被移除，所有的子实例也已经被销毁
        ```

2. data()、methods、computed等选项中定义的数据和方法，都会被转化为组件实例对象的属性和方法，可以在组件的模板和JavaScript代码中直接使用。

3. computed: 计算属性

    * 针对data数据的二次计算（直接在模板template里面也可以写运算逻辑，但是不建议，会很乱、难以维护，当计算逻辑复杂一点的时候很难写）

    ```
    <template>
        <div class="hello">
            <h1>总价： {{ total }}</h1>
        </div>
    </template>
    --------------------------------
    data() {
        return {
            price: 100,
            num: 1,
        }
    },
    computed: {
        total() {
            return this.price * this.num
        }
    }

    ```

    * 其中，total这个函数，最终会变成当前组件实例的一个属性，跟data里的变量一样，那么这里就有一个问题，如果我total函数是methods里面定义的，其实也可以实现一样的效果，只不过写到模板里面要加total()，运行一下得到返回值


    ```
    <template>
        <div class="hello">
            <h1>总价： {{ total() }}</h1>
            <h1>总价： {{ total() }}</h1>
            <h1>总价： {{ total() }}</h1>
        </div>
    </template>
    --------------------------------
    data() {
        return {
            price: 100,
            num: 1,
        }
    },
    methods: {
        total() {
            return this.price * this.num
        }
    }

    ```

    **面试题：** 注意，这里的最大区别是： computed有缓存， 如上面的代码，我total()写了三个，那么就会执行三次函数，每次都会重新计算，如果使用 computed 定义total，那么total的返回值只要不发生变化，就会一直使用缓存，不会重新计算，提高了性能

    * 前面说过，computed 属性里面的方法会转化为当前组件实例的属性，因此我们是可以通过 this.total 获取到这个计算属性 total 的，但是却不能直接进行修改，如在methods里的方法进行修改 ``this.toal = '520'`` 会报错
    
    * 如果需要对计算属性进行修改赋值操作，需要把 total 写成对象写法，定义 get、set 属性方法，在set方法里面进行相应的处理

    ```
    data() {
        return {
            price: 100,
            num: 1,
        }
    },
    methods: {
        total: {
            get() {
               return this.price * this.num 
            }
            set( val ) {
                this.price = val
            }
            
        }
    },
    methods: {
        changeTotal() {
            this.total = 1000
        }
    }

    ```

    * 注意，在上面的例子中，如果我要直接修改计算属性 total, 需要将total写成对象写法，定义 get，set 属性方法，set(val) 中的参数 val 就是要对total进行赋值的值，在 set(val) 里面，仍然不能 ``this.total = val``，需要折中处理，它只是可以监听到修改操作，并获取修改的值val参数。可以data再定义一个变量进行存放，这里我不太明白这个set的应用场景。。。应该主要是起到监听操作的效果


### 属性绑定

1. 单向数据流（v-bind）

    * vue 是 MVVM 架构的，M(model)为 script 标签的js数据, V（view）为template模板视图， VM（viewModel）为vue框架
    
    * v-bind 简写：

        ```
        v-bind:value   ===> :value
        v-bind:src     ===> :src
        v-bind:class   ===> :class

        一切属性都可以进行绑定。。。
        ```

    * M(js数据)定义数据 --> v(视图template)使用数据

**注意** 如果是单向数据流，视图修改了值，M的值不变（不会更新M）

2. 双向数据流（响应式数据：v-model 简写为 @ ）

    * M(js数据) 可以给 V(template) 使用，当 V修改数据的时候，会同时改变 M 中的数据；当 M 改变数据，V 中显示的数据也会响应式的变化。


3. 单向绑定和双向绑定的使用场景

    * 单向绑定：纯展示类的数据

    * 双向绑定：有修改或者输入等交互行为的，如要进行登录，在输入的时候，输入数据的同时改变了js中的数据，以便获取到需要的参数值



**面试题：** 

* 双向绑定：v-model

* vue中如何实现单向绑定呢？v-bind


## 七、生命周期

### 介绍

生命周期是指一个对象从诞生到死亡的一个过程，在vue中就是指一个组件从实例化诞生，到实例销毁的过程，具体看[官网](https://cn.vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

1. 先是 new Vue 实例化

2. 然后创建前后, 挂载前后，数据更新前后，组建销毁前后

3. 生命周期的书写顺序可以随便写，不会影响执行顺序，因为 Vue 源码已经规定好了各个生命周期执行的顺序了。

### 什么情况下使用哪些生命周期

* created ---> 请求接口，初始化时

* mounted ---> dom操作的时候

* updated ---> 观测数据是否更新了

* destroyed ---> 关闭(没了)：
    
    - 当用户关闭页面，但是业务上要记录一些东西的时候
    - 例如看视频网站时，当播放页播放的视频达到一定时长，用户关闭了播放页，这时候可以在这个生命周期中记录一下时长数据，当用户再次播放这条视频的时候，自动从上一次的时长开始播放。


***面试题***

this.$data ： 当前组件的data数据

this.$el : 当前组件的节点（dom，template模板下的dom）

1. 第一次进入时，执行几个生命周期？

   ``` 
   4个
   创建前后:
    beforeCreate:   没有data, 没有el
    created:        data创建了,el为undefined
   
   挂载前后
    beforeMount:    data,el为undefined（其实已经在准备了）
    mounted:        data,el创建了
    ```
    因此，在涉及到dom操作的时候，要在 mounted 生命周期及之后进行
