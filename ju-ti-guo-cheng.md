# 具体流程

### 1、创建前端App

### 运行`yarn create react-app demo-app`  将会创建一个如下图的目录：

![](/assets/import.png)

### 安装完后会有清晰的提示![](/assets/微信截图_20180205122412.png)

### 安装完成之后可以通过 `yarn start`启动   进入`src`目录开始开发即可。以下是我开发完成的app截图

![](/assets/微信截图_20180205122827.png)

### 2、关于server端

### 首先创建一个叫`server`的文件夹和初始化`package.json`文件：

1. * ### `mkdir server && cd server && yarn init`
2. ### 添加如下依赖    `yarn add`

   ![](/assets/微信截图_20180205123336.png)

   * #### 主要用到`express`,`body-parser`,`nodemon`（检测node.js 改动并自动重启，适用于开发阶段）,`babel-cli`和`babel-preset-es2015`\(以便使用 es6开发\)
   * #### ejs为后端模板引擎  其他的代码里有详述

3. ### 修改`package.json`，增加`npm scripts`

   #### `{`

   #### `"scripts": {`

   #### `"start": "nodemon --exec babel-node -- ./server.js",`

   #### `"build": "babel ./server.js --out-file server-compiled.js",`

   #### `"serve": "node server-compiled.js"`

   #### `}`

   #### `}`

   * #### 这里使用`nodemon`在开发阶段检测node.js 改动并自动重启
   * #### 发布`build`的时候则通过`babel`编译成 es5的文件

#### `create-react-app`会启动一个静态资源服务器，那么同时需要进行 server 端的时候需要怎么做呢？

#### 我们回过头来去修改一下`demo-app`目录下的`package.json`。

#### `yarn add concurrently`

#### package.json:

```
"scripts": {

    "react-start": "react-scripts start",

    "start": "concurrently 'yarn react-start' 'cd server && yarn start'",

    "react-build": "react-scripts build",

    "build": "concurrently 'yarn react-build' 'cd server && yarn build'",
},
```

#### 这样，我们只要执行`yarn start`会同步启动 webpack 以及 server文件夹下的 nodeman.

![](https://segmentfault.com/image?src=https://ws3.sinaimg.cn/large/006tKfTcgy1fgrp3bkhzzj30m80do459.jpg&objectId=1190000009857965&token=475114779f36e192fd5c171ec0b73c15)

#### Proxy

#### 如果我们在前端页面用使用`fetch(/api/data)`这样 请求，默认是会发送到create-react-app 启动

#### 的`localhost:3000/api/data`去的，无法达到目的。为了指向 server 端，需要指定`proxy`:

##### 假设 server 端 express 启动了5000端口，则需要在`package.json`中增加：

```
"proxy":"http://127.0.0.1:5000"
```

#### 这时当你使用`fetch(/api/data)`请求，则会指向到`localhost:5000/api/data`



