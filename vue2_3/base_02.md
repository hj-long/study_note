### 请求数据

在vue中，一般使用 axios 、fetch 来发起请求

#### 出现跨域问题

设置代理解决：

* vue.config.js配置: https://cli.vuejs.org/zh/config/


* 如果你的前端应用和后端 API你需要在“开发环境”下将 API 请求代理到服务器没有运行在同一个主机上，API 服务器。这个问题可以通过vue.config.js 中的 devServer.proxy选项来配置。

***注意***: 设置代理在开发环境生效，生产环境不生效!! !


* 在vue.config.js中配置代理来解决跨域的问题

```
devServer:{
    proxy: http://xxx.xxx
}
```

***注意***: 设置完代理后，一定要重启一下

#### axios的二次封装 [公司项目中基本都会做的]

* 二次封装的意义:当然有很多，目前先知道: 方便统一管理

* 在src中新建一个目录utils/request.js，然后创建请求实例


#### api的解耦[公司项目中基本都会做的]

1. api的解耦的意义： 

    * 为了同一个接口可以多次使用，那么封装起来，直接调用就可以了

    * 为了方便api请求统一的管理

2. 请求写在某一个.js文件

    * 引入axios示例，然后按照页面分类统一写请求，使用的时候，直接引入并传参数即可

***补充*** : @ 代表 src目录


***注意***

* 不管什么情况，vue的项目: axios二次封装和api解耦是一定会做的

* 请求接口: 会有2种情况

    1.前端不用处理跨域

    * 不需要设置代理来解决跨域问题

    2.前端需要处理跨域问题

    * 设置代理
        
        通过配置 vue.config.js 来解决，但是只限于开发环境，项目打包之后，代理就失效了

    * 设置环境变量(根据不同的环境来执行不同的值)

        - 创建环境变量文件,在vue项目根目录创建:

                开发环境 ---->  .env.development
                生产环境 ---->  .env.production

        - .env文件中属性名必须以 ``VUE_APP_`` 开头

        - 调用要重启 vue 项目

        - 调用 .env 文件中的变量: process.env.VUE_APP_XXX

            例如： ``console.log(process.env.VUE_APP_XXX)``

### $nextTick获取更新后的dom ( onload 完成后获取)

1. 想要获取更新后的dom : this.$nextTick

2. 在 beforeCreate 、createdbeforeMount想要获取dom，可以使用$nextTick，传入一个函数，函数逻辑内可以获取到dom元素

```
  beforeCreate() {
    this.$nextTick(() => {
      console.log('获取dom', document.getElementById('xxx'));
    })
  }
```

### ref ， 获取dom

1. 直接在元素标签上写属性 ref='xx',然后这个属性就会添加到组件实例的属性上，可以通过 ``this.$refs.xx`` 获取到对应的dom

```
<template>
  <div ref="box">123</div>
</template>
<script>
export default{
    mounted() {
        console.log(this.$refs.box);
        console.log(this.$refs['box']);
  }
}

```

2. ref 拿到的是一个dom对象，可以通过 ``this.$refs.xx`` 或者 ``this.$refs['xx']`` 拿到


### 组件

#### 概念

1. 就是把一个大的网页，拆分成多个模块，由多个模块组成一个大网页

2. 就是一个 .vue 文件

#### 管理

1. 统一放到src/components里面， 可以新建成一个个文件夹，注意引入路径就好

2. 组件的命名： 首字母要大写

3. 引入子组件，在父组件的script标签下：

    ```
    // 引入和注册
    <script>
    import Xxx from '@/components/Xxx'
        export default {
            components: {
                xxx,
            }
        }
    </script>
    // 使用组件
    <template>
        <div>
            <Xxx></Xxx>
            <Xxx />
        </div>
    </template>
    ```

#### 父组件传值给子组件

1. 父组件如何传值

    - 死数据： ``<Child msg="父组件给子的jjjbbbb"></Child>``

    - 活数据： ``<Child :msg="msg"></Child>``, 其中，msg是父组件的一个变量，而且需要使用 v-bind 进行单向绑定, 因此不可以进行修改，会报错。

        ***注意***：如果父组件传的是 <b>数组</b> 或者 <b>对象</b>，是可以修改的，但是不建议修改。

2. 子组件如何接收

    - 数组形式： 直接与data()函数同级写 ``props: ['msg']``

    - 对象形式（限定类型和默认值等）: 

        ```
            props: {
                msg: {
                    type: String,
                    default: '默认值1'
                }
            }
        ```

    - 通过 props 接收的数据， 跟data数据一样会被添加为子组件实例属性，因此可以通过 ``this.msg`` 进行获取


#### 子组件传值给父组件
    
1. 父组件定义事件并传递给子组件

    ```
    <template>
        <Child @sendMsg="sendMsg"></Child>
    </template>

    <script>
    import Child from '@/components/Child'
    export default{
    components: {
        Child
    },
    methods: {
        sendMsg(msg) {
            console.log('父组件接收:', msg);
        }
    }
    }
    </script>
    
    ```

2. 子组件通过   ``this.$emit('事件', '参数1',...)`` 触发父组件的事件，从而将子组件的信息通过参数形式进行传递，父组件中的事件就可以处理子组件的参数信息。

***注意*** 父组件传递的事件是自定义事件名称 xxo，函数为 xxoo，那就通过 ``@xx0='xxoo'`` 进行绑定传递，子组件要通过 $emit('xxo') 这个事件名称进行触发。

#### 兄弟组件之间传值（新建一个vue实例）

1. 新建工具文件 bus.js

    - 兄弟组件之间的传值可以通过一个 bus.js 来进行管理, bus.js 内容：

        ```
        import Vue from 'vue'
        export default new Vue()
        ```

    - 就是新建一个 vue 实例就可以了,然后需要引入才能使用

2. A 兄弟组件触发自定义函数并传递参数

    ```
    import bus from '@/utils/bus'
    <button @click="JJJ">兄弟</button>
    methods: {
        JJJ: function() {
            bus.$emit('busjj', this.data2)
        }
    }
    ```

3. B 兄弟组件监听自定义时间并接收参数 (可以写在任意一个生命周期内，只要执行了就能绑定事件， 因此不要放到methods方法里面，不然需要先触发方法才能绑定事件)

    ```
    import bus from '@/utils/bus'
    bus.$on('busjj', ( val ) => {
        console.log('接收：', val)
    })
    ```

***注意：***

* 这个 bus.js 通过导出一个 vue 实例来作为事件总线，本身已经脱离了组件那种依赖关系，因此不管是父子、兄弟、爷孙组件等都可以进行通信，只要引入了就可以进行事件的绑定和触发。





