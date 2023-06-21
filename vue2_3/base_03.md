## 一、路由

### 1 概念

路由 ---> 用于跳转页面的...反正是和页面打交道的

vue 中的路由有官方库 Router，点击 [官网](https://router.vuejs.org/zh/) 查看更多内容

### vue + router是单页面应用(SPA)

1. 单页面： 整个项目 ---> 只有一个html文件

2. 缺点：不适合做 SEO

3. 目前来说: 后台管理系统非常适合Vue这个框架来做

### 2 跳转页面

1. template写法: 使用 router库的组件 ``<router-link to="/about">About</router-link>`` 最后会转化为一个 a 标签

2. js 写法：

    - router.push ---> 从 a 进入 b ，也可以从 b 进入 a

        * this.$router.push('/about')

    - router.replace ---> 从 a 进入 b，但是不能返回了

        * this.$router.replace('/about')

    - router.go

    - router.back

### 3 路由模式

1. history 模式

```
const router = new VueRouter({
    mode: 'history',
    routes
})
```

2. hash 模式

```
const router = new VueRouter({
    mode: 'hash',
    routes
})
```

3. 它们的区别

- 找不到路由的情况

    - hash：找不到路由信息，会打开一个空白页（一般设置重定向），也不会发送请求

    - history：找不到路由浏览器会额外发送一次这个路由的 get 请求

- 展示形式不一样

    - hash：浏览器的 url 显示会携带一个 /#/ 符号

    - history：比较简洁，美观，符合常规的 url 形式


### 4 router-link

1. router-link 写在template部分的，用来跳转页面的

    - 写法: ``<router-link to='/跳转到对应的页面'></router-link>``

2. router-link的更多参数

    - to ==> 表示目标路由的链接(跳转到的页面)，可以通过path，也可以通过name

        ```  
        <router-link to='/course'></router-link>

        <router-link :to='{path: "/about"}'>跳转到about页面</router-link>

        <router-link :to='{name: "about"}'>跳转到about页面</router-link>

        ```

    tag ==> 可以渲染成任意标签

        - 默认不配置时，则渲染成a链接

    replace ==> 不能返回上一页和this.$router.replace一样

        <router-link to='/course' replace></router-link>


### 5 router-view

* ``<router-view />``  可以渲染路径匹配的视图组件

### 6 路由懒加载分包

1. 使用一个箭头函数运行返回组件信息，就是懒加载，其中，/* webpackChunkName: "about" */ 添加了这个webpack配置注释之后，当我们打包代码时，webpack 会对路由信息进行分包，命名为 about_xxx.js，如果没有这个注释，js文件就会变成一堆数字

```  
{
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
}
```

### 7 配置 404 页面

1. 匹配404页面实现的方法有很多，其中可以使用通配符 *

```  
import Error from '@/views/Error.vue'
{
    path: '*',
    component: Error
}
```

### 8 子路由（项目不一定需要，但是可以方便管理）

{
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
    children: [
      {
        path: '/about/page1',
        name: 'page1',
        component: () => import(/* webpackChunkName: "about" */ '../views/Page1.vue')
      }
    ],
}



### 9 动态路由

1. 动态子路由

- 在 router/index.js 中配置路由，``/course/:id`` 将url后面的字符串匹配名为 id 的路由参数

```
{
    path: '/course/:id',
    component: () => import(/* webpackChunkName: "about" */ '../components/PageNum.vue')
}

```

- 然后在展示组件 PageNum.vue 中通过 ``$route.params.xx`` 拿到路由参数进行相应的渲染

```
<template>
    <div>
        这是页面 {{ $route.params.id  }}
    </div>
</template>

```


### 10 路由守卫

点击 [router官网](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html) 查看详细内容

什么是路由守卫？

通俗解释: 就是保安大哥，ok的情况可以进入，不ok进入不了

场景: 进入某一个页面，前置性需要判断身份 (是否登录)。如果登录可以进入，如果没有登录不可以进入(跳转到登录页)

1. 路由独享守卫（写在router.js 的每一个路由信息配置内）

```
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    }
  },
]

```
各个参数含义：

- to: 即将要进入的目标

- from: 当前导航正要离开的路由 

- 可选的第三个参数：next(), 表示符合，允许跳转

```
router.beforeEach((to, from, next) => {
    if( true ) { 
        next() //符合条件，进行跳转
    }
    else {
        next('/login') //不符合条件，跳转到登录页
    }
})

```

2. 组件内的导航守卫（写在对应的组件内，但可能不好维护）

路由导航的功能不仅仅在 router 中配置，也可以直接在对应的组件内进行设置，跟生命周期的写法是一样的，一共有三个钩子：

- beforeRouteEnter
- beforeRouteUpdate
- beforeRouteLeave

3. 全局路由守卫（添加到 router.js 的路由实例的属性上）

- 全局前置守卫：``router.beforeEach((to, from)=>{...})``

- 全局解析守卫：``router.beforeResolve()``

- 全局后置钩子：``router.afterEach((to, from) => {..})``


## 二、Vuex

### 什么是 Vuex ?

是一种状态管理模式, [vuex官网](https://vuex.vuejs.org/zh/guide/)

通俗:集中式存储管理

存储的东西有哪些: 全局共享的属性、全局共享的方法.....

1. vuex的使用场景(为什么要用?)

- 很多组件共同使用某一个值的时候 (组件传值有可能很繁琐所以直接用全局共享的属性比较方便)

- 数据统一管理好维护

2. vuex 中的属性

    - state: 有点像组件的data，就是来存放共享数据的，并且是全局共享的

        - 使用方法1：组件里直接访问 ``this.$store.state.xxx``

        - 使用方法2：通过 辅助函数mapState和computed 结合解构变量

            ```
            {{ data1 }}
            <script type="javaScript">
            import { mapState } from 'vuex'
            export default {
                computed: {
                    ...mapState(['data1'])
                }
            }
            </script>
            ```

        - 上面两个的区别是？

            方式1: ``this.$store.state.xx`` 其实是在store本身里面找到的属性值

            方式2: 辅助函数的形式：mapState会把state的某一值拷贝一份给vue当前组件的实例对象上

        ***区别*** 
        
            this.$store.state.xx 可以直接修改，相当于修改源头了
        
            this.xxx 不可以直接修改

    - getters: 有点像组件的computed，放计算state属性的

        使用1: ``this.$store.getters.xxx``，同上面state类似
        
        使用2: 辅助函数的形式（mapGetter 和 computed）

        ***注意***：因为Vuex是单线数据流，所以v-model绑定getters属性会报错，而且没有get和set的写法 (组件是有的，vuex是没有的)

    - mutations: 有点像组件的methods，放全局共享方法

        定义方法：

        ```
            mutations: {
                addNum (state) {
                    state.num++
                }
            }
        ```

        使用方法1：``this.$store.commit('addNum')`` 直接使用 commit 提交方法进行运行

        使用方法2：辅助函数（mapMutations），这里是解构在 methods 里面,可以传递参数，但是只能传一个，可以用一个对象传递多个参数

        ```
            methods: {
                ...mapMutations(['addNum'])
            }
        ```

    - actions: 和 mutations 有点类似，也是存放方法的

        写法有区别(主要是参数上)：

        ```
            actions: {
                addNum ({commit, state}) {
                    state.num++
                }
            }
        ```

        使用方法1：``this.$store.dispatch("addNum")``

        使用方法2：辅助函数（mapActions）

    - modules: 把整个状态管理再次细分

        用法：

        ```
            const store = createStore({
                modules: {
                    a: moduleA,
                    b: moduleB
                }
            })
        ```

        ```
        this.$store.state.a // -> moduleA 的状态

        this.$store.state.b // -> moduleB 的状态
        ```



***方法总结***：

    mutations可以通过commit来提交
        
    actions可以通过dispatch来提交

***区别***

1. Action 提交的是 mutation，而不是直接变更状态。
    
    - actions可以直接修改state属性值? 是可以的，但是不建议这样写

    ```
    mutations: {
        addNum (state) {
            state.num++
        }
    },
    actions: {
        getNum ({commit, state}) {
            commit('addNum')
            ...
        }
    }
    ```

    ***建议是使用 mutations 的 commit 触发提交去修改 state 的状态***

2. 同步/异步
    - mutations是同步函数
    
    - actions 是可以包含异步的


### Vuex 持久化存储

**注意: Vuex是一个集中式的状态管理工具，本身不是持久化存储，如果要实现持久化存储可以:

1. 自己写localStorage（麻烦）

```
  state: {
    num:  localStorage.getItem('num') || 1
  },
  mutations: {
    add( store ) {
        store.num++;
        localStorage.setItem('num', store.num);
    }
  }
```

2. 使用插件（方便）

    - 安装 ``npm install vuex-persistedstate -s``

    - 引入并配置

    ```
    import persistedState from 'vuex-persistedstate'
    let create_persistedstate = persistedState({ 
        key:'store', //用于存储持久状态的密钥。默认为vuex
        //代替（或结合）getState和setState。默认为localStorage。
        storege:window.loaclStorege
    })
    export default new Vuex.Store({
        // ...
        plugins: [create_persistedstate]
    })
     ```


***面试题***

面试题一: 当某一个组件使用了vuex的数据，比如把1修改成了2，刷新页面又到了1，该怎么办?

1. Vuex是一个集中式的状态管理工具，本身不是持久化存储，如果要实现持久化存储可以通过 localStorage 或者 插件 来实现持久化的效果


面试题二:在某个组件中可以直接修改Vuex的状态(数据) 吗?

1. 可以的情况：

    - 通过mutations方法来修改
        
    - 组件内直接修改vuex数据源 ``this.$store.state.shop.num = 200;``

2. 不可以的情况

    - 直接使用辅助函数 ``this.num=200`` 这种情况是不可以修改的

面试题三: Vuex中的getters属性在组件中被v-model绑定会发生什么?

1. 绑定本身不会报错，如果修改了，会报错，因为Vuex是单向数据流。

面试题四: Vuex是单向数据流还是双向数据流?

1. Vuex是单向数据流。


## 三、插槽

插槽就是一个子组件在模板上预留了一个位置，当引用这个组件的父组件插入了dom时，子组件会将父组件的dom放在合适的地方进行展示

1. 匿名插槽

- 插槽没有名字

    父组件：
    ```
    <Child>
        <div>123</div>
    </Child>
    ```
    子组件：
    ```
    <template>
        <slot></slot>
    </template>
    ```

2. 具名插槽

- 插槽有名字，可以对应放到某一个位置上

    父组件(要使用 template 包着)：
    ```
    <Child>
        <template #header>
            <div>123</div>
        </template>

        <template #footer>
            <div>456</div>
        </template>
    </Child>
    ```
    子组件：
    ```
    <template>
        <slot name='header'></slot>
        jjj
        <slot name='footer'></slot>
    </template>
    ```

3. 作用域插槽

- 可以传值

    父组件(要使用 template 包着)：
    ```
    <Child>
        <template #header='{user}'>
            <div>123{{ user.name }}</div>
        </template>
    </Child>
    ```
    子组件：
    ```
    <template>
        <slot name='header' :user='user'></slot>
    </template>
    ----------------------------
    data() {
        return {
            user: {
                name: 'xxx', age: 18
            }
        }
    }
    ```


## 四、其他

### 查找父组件和子组件

1. 查找父组件

    ``this.$parent`` ---- 返回当前组件的父组件实例

    ``this.$root`` ---- 根组件(如果当前实例没有父实例，此实例将会是其自己)

2. 查找子组件

    ``this.$children`` ---- 返回当前组件的所有子组件的实例数组（通过数组下标获取单个子组件实例）
 
        可以通过 ``this.$children[0].xxx = 'xxxxx`` 去修改子组件的值！

    ***面试题***: 父组件想要直接修改子组件的数据怎么办?

    - this.$children[0].xxxx =aaaaa'; 
    - 父组件使用 ref 获取子组件的 dom ，然后进行修改


### this.$set

1. 使用场景:

    当修改一个响应式数据的时候，数据本身改变了但是视图没有进行更新使用$set来更新

2. 语法:
        
        this.$set( 对象或者数组 ，修改的key或下标 ，修改后的值 );


### 依赖注入(provide/inject)

某组件可以直接让它下面的组件传值 (没有组件的父子限制)

1. 场景:顶层组件给后代组件传值

2. 用法：

    - 父组件提供依赖 provide

    ```
    provide () {
        return {
            str: '我是父组件里的数据'
        }
    }
    ```

    - 子组件注入使用 inject

    ```
    inject: ['str']

    ```

### 混入（mixins）

mixins 是一个 js 文件，作为 全局属性、全局方法， 只要导入了就能使用

1、创建 mixins.js 文件，导出一个变量，这个变量写法跟我们组件里的script写法一样

```
export const m = {
    data () {
        return {
            number: 1
        }
    }, 
    methods: {
        addNumber () {
            this.number++
        }
    }
}
```

2、导入 mixins 方法并使用，跟正常的 data、methods 属性一样使用

```
<script>
import { m } from '../mixins.js'
export default {
    mixins: [m],
}
</script>
```

上面的 mixins 跟正常组件的js使用方法一样，但是这就带来了一些问题，就是 数据 和 方法 的源头不好追踪了，可能造成维护困难等情况



3、混入和vuex的区别

    - 混入功能:  有点像工具类，所以是全局的，可以导出全局属性和方法
    
    - vuex功能: 状态管理==》vuex强调的是管理状态（state数据 ）

4、属性的区别:

    - 混入的属性他们是不互相影响的 (不共享)
    
    - Vuex中的属性(state)是全局共享

5、方法的区别:
    
    - 混入的方法是可以return （直接执行一个方法，return 字符串等数据，可以展示到页面上）

    - Vuex的方法是不可以return的（return 的数据无法展示到页面上）
        
