## 学习平台

### 一、创建项目

#### 使用vite创建项目 ``npm create vite@latest``

#### 安装 setup 自动导入： ``npm install unplugin-auto-import -D``

- 在 vite.config.js 中配置

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

#### 安装 vue-router：``npm install vue-router@4``

- 新建文件夹 router/index.js



- 在 mian.js 中配置

#### 安装 elementUI-plus：``npm install element-plus --save``

- 按需引入插件： ``npm install -D unplugin-vue-components``

- vite.config.js 配置：

```
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  plugins: [
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})

```

