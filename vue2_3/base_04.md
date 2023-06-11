## 一、vite

为什么要使用vite

    相比于 webpack ，更快，提高效率



## 二、vue3

### vue3 对比 vue2 有什么区别

1. 组合式API

使用 (data、computed、methods、watch) 组件选项来组织逻辑通常都很有效。然而，当我们的组件开始变得更大时，逻辑关注点的列表也会增长。尤其对于那些一开始没有编写这些组件的人来说，这会导致组件难以阅读和理解。


### setup 写法

1. 定义数据

    - let str ='1';  --->  死数据，不可以修改之类的，但是可以展示在视图层

    - 响应式数据 : ref

        - 在使用的时候需要: X.value3

    - 响应式数据: reactive

        - 在使用的时候需要不像ref一样，不需要.value
        
        - 但是 reactive 只能写对象或者数组

2. vue2 和 vue3 数据拦截不同点

    - vue2.x --->  使用 Object.defineProperty

        - 不能监听数组的变化
        - 必须遍历对象的每一个属性

    - vue3.x --->  使用 new Proxy

        - 而 Proxy 不需要遍历

3. setup语法糖插件

    解决 ``import { ref, reactive } from 'vue'`` 引入的问题

    - 下载安装：``npm install unplugin-auto-import -D`` 

    - 在 vite.config.js 中进行配置：

        ```
        import AutoImport from 'unplugin-auto-import/vite'

        export default defineConfig({
        plugins: [
            vue(),
            AutoImport({
            // 插件支持哪些库免导入
            imports: ['vue', 'vue-router','你的库的名字'],
            })
        
        ],
        })
        ```

4. toRefs 解构

toRef 一般用于解构 reative 的响应式变量

```
const obj  = reactive({
    name: '张三',
    age: 18
})
const { name, age } = toRefs(obj)

```

这时候的 name、age 就可以作为一个响应式变量来使用了，在模板上直接 name\age，修改则要name.value='xxx'

5. computed 计算属性

- 写法1（直接传一个函数，默认为 get() ）

    ```
    let msg = computed(() => {
        return name.value + ' ' + age.value
    })
    ```

- 写法2（传一个对象，自定义 get()、set() ）

    ```
    let msg = computed({
        get() {
            return 'hello'
        },
        set(val) {
            console.log(val)
        }
    })
    ```

6. watch 监听函数

- 监听某一个

    ```
    const str = ref('hello world')

    watch(str, (newVal, oldVal) => {
        console.log(newVal, oldVal)
    })

    ```

- 同时监听多个

    ```
    const str = ref('hello world')
    const str1 = ref('hello world1')

    watch([str, str1], (newVal, oldVal) => {
        console.log(newVal, oldVal)
    })

    ```

- 初始化监听（immediate: true）：在页面加载时，自动监听一次

    ```
    const str = ref('hello world')

    watch(str, (newVal, oldVal) => {
        console.log(newVal, oldVal)
    }, { immediate: true })

    ```

- 监听对象

    ```
    watch( obj, (newVal, oldVal) => {
        console.log('obj', newVal, oldVal)
    })
    ```

- 监听对象的某一个key，并且深度监听（deep: true）

    ```
    watch(() => str.name, (newVal, oldVal) => {
        console.log('name', newVal, oldVal)
    },
    {
        deep: true
    })
    ```

- 立即监听函数

    ```
    watchEffect(() => {
        console.log('watchEffect', str.value)
    })
    ```


### 生命周期钩子

如果使用选项式的，就跟 vue2 的写法一样，如果使用 setup 写法，则在每个生命周期函数前面加上个 on





#### 路由

router4.x 

1. 区别
  
   tag 属性去除了，`` <router-link to='/about' tag='div'>跳转到关于我们</router-link>``，也就是不能像 vue2 使用 router 一样将这个标签渲染成其他指定标签，只会渲染成 a 标签

2. 写法区别

    let router = useRouter(); ====》 this.$router
    
    let route = useRoute();   ====>  this.$route

3. 导航守卫还是一样


### 组件

#### 1. 父传子

- 父组件传值跟 vue2 一样，只是子组件变成了 defineProps，下面是三种写法：

    ```

    defineProps({
        msg: String,     //普通对象形式
    })

    defineProps(['msg'])   // 数组形式

    defineProps({
        msg: {
            type: String,
            default: 'hello world'    //带默认值
        },
    })

    ```

- 也可以通过非 setup 写法，在 setup 函数里面使用 prop 参数

    ```
    export default {
        props:['msg'],
        setup( prop ) {
            let msg = prop.msg + ' 123'
            return {
                msg
            }
        },
    }

    ```

#### 2. 子传父

- 在vue2中，先是 父组件使用 v-vmodel 传递一个函数给子组件 ``@getdata='fatherEvent'``， 然后子组件内使用 this.$emit('getdata', '参数1') 进行触发，并将信息通过参数形式传递给父组件

- 在vue3 中，子组件接收事件可以从 setup 函数第二个参数进行接收，setup 第二个参数是一个上下文对象 context，里面包含了 attrs, slots, emit, expose 等属性，可以解构出来使用：

    ```
    export default {
        emits: ['getStr'],
        setup( prop , { emit }) {
            const toStr = () => {
                emit('getStr', '子组件的值')
            }
            return {
                toStr
            }
        },
    }
    ----------*****或者可以使用methods*****--------

    export default {
        emits: ['getStr'],
        methods: {
            toStr() {
                this.$emit('getStr', '子组件的值')
            }
        }
    }
    ```

- 使用 setup 语法糖，就可以直接使用 defineEmits 来接收和触发事件

    ```
    <script setup>
        const emit = defineEmits(['getStr'])
        const toStr = () => {
            emit('getStr', '子组件数据111')
        }
    </script>

    ```

- 父子组件使用 v-vmodel 双向绑定传值

    - 父组件使用 v-model，注意并不能直接使用缩写 @

    ```
        <Children v-model:num="num"/>
    ```

    - 子组件使用 defineProps 接收，还可以使用 defineEmits(['update:num']) 进行修改

    ```
    const prop = defineProps(['num'])
    const emit = defineEmits(['update:num']) // 'update:num' 固定写法

    //通过事件进行修改父组件的值
    const changeNum = () => {
        emit('update:num', '修改的值')
    }

    ```

#### 3. 兄弟组件之间传值

- 方法1：两个兄弟组件之间，将值传给父组件，由父组件充当桥梁进行通信，这种方法只用到 props、emit 就可以实现，但缺点是比较实现方法比较繁琐、代码量较大

- 方法2：使用 bus.js

- 方法3：插件 mitt , 原理也是使用 bus.js

    - 下载安装 ``npm install mitt -S``
    - 新建文件 plugins/Bus.js

        ```
        import mitter from 'mitt'
        const bus = mitter()
        export default bus
        ```

    - 兄弟组件 A 引入传递事件
        
        ```
        import bus from '../plugins/Bus.js'
        const str = ref('A组件的值')
        const toStr = () => {
            bus.emit('fn', str)
        }
        ```

    - 兄弟组件 B 引入并注册使用事件

        ```
        import bus  from '../plugins/Bus.js'
        const str = ref('B组件的值')
        bus.on('fn', (val) => {
            str.value = val
        })
        ```

- 方法还有很多。。。


#### 4. 插槽

- 匿名插槽

    - 父组件，直接写dom：``<Child><span>这是插槽内容</span></Child>``

    - 子组件，定义插槽占位标签 ``<slot></slot>``

        ```
        <template>
            <p>Child组件</p>
            <slot></slot>
        </template>
        ```

- 具名插槽

    - 父组件，需要将插槽内容用 template 模板包起来，还要进行命名 v-slot: xxx,如：``<template v-slot:A1></template>``, 也可以简写，直接写成 #A1, 如：``<template #A1></template>``

    - 子组件，使用 slot 标签 和 name 属性 ``<slot name="A1"></slot>``


- 作用域插槽（插槽可以访问到子组件内的数据）

    - 父组件，使用模板和 v-slot 进行绑定子组件的变量，得到的是一个对象，可以通过解构获取到子组件传来的对象：
    
        ```
        <template v-slot:A1="{data}">
            this A1 的传来的：{{ data.name }}
        </template>
        ```

        具名作用域插槽：``v-slot:A1="{data}"``

        具名插槽的简写： ``#A1="{data}"``

        匿名作用域插槽：``v-slot="{data}"``

        ***注意***，如果不使用 {} 进行解构，那么使用的时候，就需要 ``data.data.xxx``

    - 子组件传递数据（v-bind 或者 ：）

        ```
        <template>
        <slot name="A1" :data="str1"></slot>
        </template>

        <script setup>
        const str1 = {
            ggg: 'A组件的值',
            age: 18
        }
        </script>
        ```

- 动态插槽

    - 说了就是通过父组件的数据进行切换要再子组件中展示的位置

    - 父组件

        ```
        <template #[name]>this name</template>

        //定义变量（A1 或者 A2 或者 A3 ..）

        let name = ref('A1')

        ```

    - 子组件

        ```
        <template>
            <p>A组件</p>
            展示A1:
            <slot name="A1"></slot>
            <hr />
            展示A2:
            <slot name="A2"></slot>
        </template>

        ```
    
    - 当父组件的 name 的值为 A1 的时候，插槽内容在子组件的 A1 slot标签处渲染，name 的值为 A2 时，渲染在 A2 slot标签位置上

- Teleport 传送

    - 就是把组件里某段 dom 渲染到指定位置

    - 有三种形式，id、class类、tag标签名

        ```
        <teleport to='#container'></teleport>

        <teleport to='.main'></teleport>

        <teleport to='body'></teleport>
        
        ```

    ***注意*** 必须传送到已有 dom 的内容，因此要注意顺序，也就是要渲染的 位置 不能比 传送的 要晚渲染，否则会失败

    - 使用场景：一些组件里面的展示类dom，需要放大全屏展示，这时候就可以使用 Teleport 把展示的内容传送到 body 等地方了，这样就不会收到组件内部的定位、宽高等影响导致放不大。



#### 5. 动态组件

- 可以通过 Vue 的 <component> 元素和特殊的 is attribute 实现动态组件切换，就是把组件当成一个变量来赋值进行切换。

    ``<component :is="currentTab"></component>`` 

    其中，:is 的值可以是：

    - 被注册的组件名
    - 导入的组件对象

    用法：

        ```
        <template>
            <button @click="changeCom">切换组件</button>
            <keep-alive>
                <component :is="currrentCom.com"></component>
            </keep-alive>
        </template>

        <script setup>
            import A from './A.vue';
            import B from './B.vue';

            const comList = reactive([
                { name: 'A',com: markRaw(A) },
                { name: 'B',com: markRaw(B) }
            ])

            const currrentCom = reactive({
                com: comList[0].com
            })

            const changeCom = function() {
                if(currrentCom.com === comList[0].com) {
                    currrentCom.com = comList[1].com
                } else {
                    currrentCom.com = comList[0].com
                }
            }
        </script>

        ```

    - 定义一个组件列表 comList，存放引入的组件对象，在定义一个 currrentCom 保存当前组件的指针，并将其绑定到 <component> 标签的 :is 属性上成为一个动态组件，接着一个按钮，负责切换组件对象。

    - 当使用 <component :is="..."> 来在多个组件间作切换时，被切换掉的组件会被卸载。我们可以通过 <KeepAlive> 组件强制被切换掉的组件仍然保持“存活”的状态。


#### 6. 异步组件

可以提升性能，在需要的时候再去加载...

- 使用 defineAsyncComponent 方法来实现此功能

    ```
    import { defineAsyncComponent } from 'vue'

    const Child = defineAsyncComponent(() => {
        import('./components/Child.vue')
    })

    ```

- 使用场景

1. 组件按需引入: 当用户访问到了组件再去加载该组件

    - 可以结合 [vueuse](https://vueuse.org/core/useIntersectionObserver/#useintersectionobserver) 来使用: ``npm i @vueuse/core``

    ```

    <template>
        <div ref='target'>
            <Child v-if='targetIsVisible'></Child>
        </div>
    </template>

    import { useIntersectionObserver } from '@vueuse/core'

    const target = ref(null)
    const targetIsVisible = ref(false)

    const { stop } = useIntersectionObserver(
      target,
      ([{ isIntersecting }]) => {
        //当用户滑到 target 为可视区域时，isIntersecting为true
        if(isIntersecting) { 
            // 为true则让dom加载
            targetIsVisible.value = isIntersecting 
        } 
      },
    )

    ```

2. 显示异步组件加载中的状态（Suspense）

- ``<Suspense>`` 组件有两个插槽：#default 和 #fallback。两个插槽都只允许一个直接子节点。在可能的时候都将显示默认槽中的节点。否则将显示后备槽中的节点。

    ```
    <Suspense>
        <template #default>
            <Child></Child>
        </template>
        <!-- 在 #fallback 插槽中显示 “正在加载中” -->
        <template #fallback>
            正在加载中...
        </template>
    </Suspense>

    ```

3. 好处：使用异步组件，可以在打包时进行分包处理，避免组件全部在一个大的js文件中，在需要时再进行加载，提高了性能

#### 7. Mixin 混入

可以分发 vue 组件中可复用的功能

1. 创建 mixins.js 文件，写一个可复用的收藏按钮逻辑，逻辑是，点击按钮，按钮的字体变成 收藏中... ，过了2秒之后，变回 收藏 两个字，同时，计数加1

    ```
    import { ref } from 'vue';

    export default function() {
        let num = ref(0);
        let flag = ref(false);

        let btnFav = () => {
            num.value++;
            flag.value = true;
            setTimeout(() => {
                flag.value = false;
            }, 2000)
        }
        
        return {
            num, flag, btnFav
        }
    }

    ```

2. 在组件中引入并使用(数据不共享)

    - 引入，运行函数 mixin()，通过解构可以方便拿到变量

    ```
    <template>
        {{ num }}
        <button @click="btnFav">
            {{ flag ? '收藏中...' : '收藏' }}
        </button>
    </template>

    <script setup>
        import mixin from '../mixin/mixins.js'
        let { num, flag, btnFav } = mixin();
    </script>

    ```

#### 7. 依赖注入 provide\inject

- 组件，提供数据 ``provide('data', '100')``

- 后代组件，注入使用 ``let data = inject('data')``


#### 8. vuex

- 和 vue2.x 中有什么区别？

1. 依然是:

- state（data变量）
    
- getters（计算属性）
    
- mutations（方法,修改state）: 提交 ``store.commit('xx', param)``
    
- actions（支持异步方法，请求）：调用 ``store.dispatch('xx')``
    
- modules（分模块使用，多一层）：``useStore().store.num``


在 vue3.x 中，如果使用选项式写法，则跟 vue2.x 的没什么区别，如果采用 setup 写法，则需要使用 computed 处理获取的值，不然会失去响应式

```
import { useStore } from 'vuex'

let store = useStore()
//获取变量
let num = computed( () => (store.state.num) )
//获取'计算属性'
let total =  computed( () => (store.getters.total) )
//获取修改函数进行commit触发
let btn = () => {
    store.commit('btn', 10);
}

```

2. vuex 持久化存储

- 也是一样，要么使用 localStorage，要么使用插件


#### 9. Pinia

也是一个状态管理工具， [vuex和pinia的区别](https://github.com/vuejs/rfcs/pull/271)

大致总结

    1.支持选项式api和组合式api写法

    2.pinia没有mutations，只有: state、getters、actions

    3.pinia分模块不需要modules (之前vuex分模块需要modules)

    4.TypeScript支持很好

    5.自动化代码拆分

    6.pinia体积更小(性能更好)

1. 使用

    - [官网](https://pinia.vuejs.org/zh/introduction.html) 下载安装：``npm install pinia``

    - main.js 引入：

        ```
        import { createPinia } from 'pinia'

        const pinia = createPinia()
        createApp(App).use(pinia)

        ```

    - 创建 ./src/store/index.js, 首先引入 defineStore 

        ```
        import { defineStore } from "pinia";

        export const useStore = defineStore( "store", {
            state: () => ({
                // 定义data
            }),
            getters: {
                // 定义计算属性
            },
            actions: {
                // 定义 方法，支持异步
            }
        })
        ```


    - 组件中引入，pinia中的state数据可以直接修改

        ```
        import { storeToRefs } from 'pinia';
        import { useStore } from '../store';

        const store = useStore();
        //直接解构得到的变量不是响应式的
        //let { name, age } = store;
        // 使用 storeToRefs 可以得到响应式变量
        let { name, age } = storeToRefs(store);

        //批量更新
        store.$patch( state => { 
            state.num++;
            state.name = '炸雷';
        })

        ```

    - getters 计算属性，有缓存

        ```


        ```

    - 批量更新 ``store.$patch``,可以同时修改state中的变量

        ```
        store.$patch( state => {
            state.xx1 = 'xxx';
            state.xx2 = 'xxxx';
        })

        ```

    - 重置 ``store.$reset``, 可以重置已经修改的数据


2. 模块划分

- pinia 的模块划分非常简单，只需要新建一个文件，使用不同的变量导出就可以了

例如，定义 store/user.js、store/shop.js ,以及一个唯一的ID 就可以完成：

    ```
    import { defineStore } from "pinia";

    export const store名字 = defineStore( "唯一ID", {
        state: () => ({
            //
        })
    });

    ```

- 组件中引入使用

    ```
    import { storeToRefs } from 'pinia';
    import { shop } from '../store/shop.js';
    import { user } from '../store/user.js';
    // 拿到不同 pinia 模块的实例
    const shopStore = shop();
    const useStore = user();
    // 解构响应式变量
    let {xx, yy} = storeToRefs(shopStore);
    let {xx, yy} = storeToRefs(useStore);
    // 使用不同模块的方法
    shopStore.xxfn();
    useStore.xxfn();

    ```

3. pinia 持久化存储

    [参考链接](https://xuexiluxian.cn/blog/member/detail/acebacd99612447e8c80dcf6354240f6)

- 使用 localStorage 

- 使用插件：``npm i pinia-plugin-persist --save``


#### 10. vite 设置代理

[参考文章](https://xuexiluxian.cn/blog/detail/01f62baa85b7431992586b4689a9b07a)

1. 在 vite.config.js 中配置即可

    ```
 	server:{
		proxy:{
			'/api':'http://xxxxxx'
		}
	}

    ```

2. axios 二次封装





