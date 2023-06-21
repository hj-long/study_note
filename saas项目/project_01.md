# 学习平台

## 一、创建项目

### 使用vite创建项目 ``npm create vite@latest``

### 安装 setup 自动导入： ``npm install unplugin-auto-import -D``

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

### 安装 vue-router：``npm install vue-router@4``

- 新建文件夹 router/index.js



- 在 mian.js 中配置

### 安装 elementUI-plus：``npm install element-plus --save``

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


### 关于Vite不能使用require问题

  - 安装插件： ``npm i vite-plugin-require-transform --save-dev``

  - vite.config.js 配置：

    ```
    import { defineConfig } from 'vite'
    import requireTransform from 'vite-plugin-require-transform';
    
    export default defineConfig({
      plugins: [
        requireTransform({
          fileRegex: /.js$|.vue$/
        }),
      ],
    });

    ```

    - [vite 官网解决方案](https://cn.vitejs.dev/guide/assets.html)



## 二、登陆逻辑

登录方式有三种，无论哪种，在发送登陆请求成功之后，后端会返回一个 token，前端需要把这个 token 存储到 pinia 并且做持久化存储


### 账号密码登录

1. 账号密码登录需要对登录的账号和密码进行一个加密，这里采用 ``crypto-js``

2. 安装：``npm i crypto-js``

3. 使用：

```
import CryptoJS from "crypto-js";

const key = CryptoJS.enc.Utf8.parse("AOWQ4P0YEC4YXUKS");  //十六位十六进制数作为密钥
const iv = CryptoJS.enc.Utf8.parse('O3V2GCL1K2HNZ9Y7');   //十六位十六进制数作为密钥偏移量

//解密方法
export function Decrypt(word) {
    let encryptedHexStr = CryptoJS.enc.Hex.parse(word);
    let srcs = CryptoJS.enc.Base64.stringify(encryptedHexStr);
    let decrypt = CryptoJS.AES.decrypt(srcs, key, { iv: iv, mode: CryptoJS.mode.CBC, padding: CryptoJS.pad.Pkcs7 });
    let decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);
    return decryptedStr.toString();
}

//加密方法
export function Encrypt(word) {
    let srcs = CryptoJS.enc.Utf8.parse(word);
    let encrypted = CryptoJS.AES.encrypt(srcs, key, { iv: iv, mode: CryptoJS.mode.CBC, padding: CryptoJS.pad.Pkcs7 });
    return encrypted.ciphertext.toString().toUpperCase();
}

```

  在使用时，先需要对表单进行校验，确定得到了正确格式的账号和密码之后，发送登陆请求，请求参数加密：

  ```
  //加密
  import { Encrypt } from '../utils/aes'

  username = Encrypt(ruleForm.username)
  userpwd = Encrypt(ruleForm.userpwd)

  ```

  然后就是调用账号密码登陆接口进行发送请求~

### 第三方登录

暂无。。

### 短信登录

在短信登录时，需要使用滑块验证组件

1. 输入正确手机号：前端判断,通过表单校验，确定得到正确格式的手机号码

2. 输入正确手机号后，出现滑块，滑块验证通过才可以倒计时发送短信api

3. 手机号和验证码都正确后,调用短信登陆接口发送请求


### 退出登陆

1. 当用户点击退出登陆时，需要提示用户是否确定退出登陆，当用户确定退出登陆时，我们需要清除 pinia 中的 token 和用户信息，然后根据需要跳转页面


### 下载资料

1. 点击下载按钮

2. 查询是否登录状态，如果登录正常走下面流程，如果没有登录，跳转到登录页

    - 先检查是否有 token，没有则提示用户并跳转登录页

3. 如果用户登陆登陆，调用接口查询是否有权限下载，（比如是否购买了本课程）

4. 如果无权限提示“需要购买课程”，如果有权限直接进入下载

5. 调用下载接口，通过blob流下载（因为后端返回的是文件流）

    - 下载的接口需要加上参数：``responseType: 'blob'``

    - 然后在请求方法 then 里面写以下逻辑：

    ```
    //将服务端返回的二进制流文件转换为Blob对象
    const blob = new Blob([res]);
    //获取文件名
    let fileName = item.attachmentName;
    //获取文件URL
    let fileUrl = item.attachmentUrl;
    //获取文件扩展名（后缀）
    const extName = fileUrl.substring(
          fileUrl.lastIndexOf('.')
    )
    //完整文件名
    fileName = fileName + extName;
    // 创建a标签，添加一个download属性来指定下载文件名
    const link = document.createElement('a');
    link.download = fileName;
    //_blank，下载链接会在新窗口中打开，防止跳转
    link.target = '_blank';
    //隐藏
    link.style.display = 'none';
    //将Blob对象转换为URL，并将其赋值给a标签的href属性
    link.href = URL.createObjectURL(blob);
    document.body.appendChild(link);
    模拟用户点击下载链接
    link.click();
    //移除标签，清理内存
    URL.revokeObjectURL(link.href);
    document.body.removeChild(link);
    ```

***注意***：为什么要将下载的文件转化为blob流再通过a标签下载保存呢？

1. 可以指定文件名称：如果直接从服务器传输文件，通常情况下，文件名是由后端代码指定的。转为blob流，前端可以控制文件的名称

2. 可以灵活控制下载：如果直接从服务器传输文件，通常情况下，下载的方式和行为是由浏览器决定的，例如，图片文件下载时会直接跳转新窗口进行打开预览了，影响一定的用户体验，而通过blob，前端可以更灵活控制下载文件的方式和行为

3. 降低服务器的负载


## 三、视频播放

1. 需要下载[播放器插件](https://github.com/xdlumia/vue3-video-play )

2. 设置视频防盗链

    设置了防盗链Referer之后，只有指定域名的请求才能获取到视频数据，这样可以防止一些非法访问

3. 轮询更新播放记录

    通过每隔一段时间，发送视频播放记录的请求，记录用户播放的时长，当用户突然关闭视频再重新打开的时候能够自动从上次离开的时间继续播放


## 四、支付流程

通常先加入 商品 到购物车，然后在购物车页面点击 结算 按钮，跳转 订单页面 ，此时一般可以选择支付方式 微信 或者 支付宝，然后点击 支付，跳出 支付二维码，最后是完成支付

1. 点击``去结算``按钮，调用``结算接口``，参数一般为 ``购物车的商品ID列表(商品id，数量)``，返回``支付方式``等参数，跳转 ``确认订单`` 页面

2. 在订单页面，点击选择 ``支付方式（支付宝、微信等）``，这时候就以``商品列表、支付方式``等参数调用对应的``生成支付订单``接口，返回``订单号``、``支付二维码图片url``，最后，渲染显示 二维码 图片的``弹窗``即可

3. 此时，每隔 2s 不断轮询``查询订单状态``接口，检查订单状态，如果返回 ``支付成功``，就可以结束轮询，进行下一步 ``跳转首页`` 等操作

总结一下：

  - 用户选择商品，加入购物车，在购物车页面点击进入结算页面

  - 用户选择一种支付方式，前端发送支付请求，参数为：订单信息和支付方式

  - 后端返回 支付页面 链接或者 支付二维码，前端将其展示

  - 用户进行支付，前端轮询支付状态，成功则下一步

