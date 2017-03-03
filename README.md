# Node 第二天课程笔记

## 0. 反馈

- 感觉慢
- 选好nodejs能让我们飞么
  + 没有银弹
- 感觉听了一天 就像上语文课
- 调试没听懂
- alias与paths有什么不同，区别是什么？
- 10月15号的时候Facebook推出了Yarn，听说代码和node差不多，只是Yarn可以离线下载各种包，这个就发展来看以后会不会替代node呢，有空给我们讲讲呗，普及下知识
  + Yarn 是一个类似于 npm 的一个东西
  + 可以用来安装和管理包
  + 只是一个工具而已

---

## 1. Node 中的 JavaScript

- ECMAScript
- Class：Buffer
- 定时器函数和清除定时器函数
- global
- process
- console
- 模块内成员
  + __dirname 和 __filename
  + exports
  + module
  + require

---

## 2. 模块系统

### 2.1 模块系统解决方案

Node 基于 CommonJS 规范实现了自己的一套模块系统规范。

CommonJS 不仅仅包含 模块定义规范，它还给 JavaScript 语言没有触及到服务器领域制定了一些规范。
例如：二进制数据操作、文件操作、网络操作、进程操作、模块系统规范。。。

- module.exports
- exports
- requrie

Node 中的模块，一共分为三类形式，具体如下：

### 2.2 文件模块

以 `./` 或 `../` 开头的模块标识就是文件模块，一般就是用户编写的。

### 2.3 核心模块

- 核心模块就是 node 内置的模块，需要通过唯一的标识名称来进行获取。
- 每一个核心模块基本上都是暴露了一个对象，里面包含一些方法供我们使用
- 一般在加载核心模块的时候，变量的起名最好就和核心模块的标识名同名即可
  + 例如：`const fs = require('fs')`
- 核心模块本质上也是文件模块
  + 核心模块已经被编译到了 node 的可执行程序，一般看不到
  + 可以通过查看 node 的源码看到核心模块文件
  + 核心模块也是基于 CommonJS 模块规范

每一个模块都提供了单一的功能，以下是常用的核心模块：

- fs            文件操作
- http          http服务
- net           Socket网络编程
- os            操作系统相关
- path          路径操作
- querystring   处理查询字符串
- url           处理url路径
- util          工具函数

### 2.4 第三方模块

一般就是通过 `npm install` 安装的模块就是第三方模块。

加载规则如下：

- 如果不是文件模块，也不是核心模块
- node 会去 node_modules 目录中找（找跟你引用的名称一样的目录），例如这里 `require('underscore')`
- 如果在 node_modules 目录中找到 `underscore` 目录，则找该目录下的 `package.json` 文件
- 如果找到 `package.json` 文件，则找该文件中的 `main` 属性，拿到 main 指定的入口模块
- 如果过程都找不到，node 则取上一级目录下找 `node_modules` 目录，规则同上。。。
- 如果一直找到代码文件的根路径还找不到，则报错。。。

注意：对于第三方模块，我们都是 `npm install` 命令进行下载的，就放到项目根目录下的 `node_modules` 目录。

---

## 3. 包与npm

### 3.1 包

包其实就是一些模块组织到一起，放到一个目录中的一个称呼，叫包或者模块都行。

### 3.2 package.json

包说明文件。

### 3.3 npm

- 本地安装
  + `npm install 包名 [--save]`
  + 本地安装一般用于项目的功能开发，我们会主动的去加载这个模块包，然后使用里面提供的一系列功能
  + 例如 underscore
- 全局安装
  + 全局安装一般是安装一些工具性的东西
  + `npm install 包名 --global|-g`
  + less
  + http-server
    * 可以实现快速启动一个服务器
  + browser-sync
    * https://www.browsersync.io/
    * 前端可视化开发
  + nodemon
    * 可以实现文件代码改变自动重启
  + 使用 `nom root -g` 可以查看全局命令行工具安装的路径

以上这些东西只是片面的，最主要就是要学会搜索以及查看文档。

---

## 4. 文件操作

### 4.1 path

- path.basename(path[, ext])：获取文件名部分
- path.dirname(path)：获取目录部分
- path.extname(path)：获取扩展名部分
- path.isAbsolute(path)：判断是否是绝对路径
- path.join([...paths])：将多个路径拼接为一个路径

### 4.2 同步调用和异步调用

fs模块对文件的几乎所有操作都有同步和异步两种形式，例如：`readFile()` 和 `readFileSync()`。

同步与异步文件系统调用的区别

- 同步调用立即执行，会阻塞后续代码继续执行，如果想要捕获异常需要使用 `try-catch`
- 异步调用不会阻塞后续代码继续执行，需要回调函数作为额外的参数，通常包含一个错误作为回调函数的第一个参数
- 异步调用通过判断第一个err对象来处理异常
- 异步调用结果往往通过回调函数来进行获取

Node 只在文件IO操作中，提供了同步调用和异步调用两种形式，两者可以结合使用，
但是推荐能使用异步调用解决问题的情况下，少用同步调用。

### 4.3 文件操作常用API

- fs.writeFile(file, data, callback)：文件写入
- fs.appendFile(file, data, callback)：文件追加
- fs.readFile(file[, options], callback)：文件读取
- fs.unlink(path, callback)：删除文件
- fs.stat(path, callback)：获取文件信息
- fs.access(path, callback)：验证文件路径是否存在
- fs.rename(oldPath, newPath, callback)：重命名或移动文件

### 4.4 目录操作常用API

- fs.mkdir(path, callback)：创建一个目录
- fs.rmdir(path, callback)：删除一个空目录
- fs.readdir(path, callback)：读取一个目录
- fs.rename(oldPath, newPath, callback)：重命名或移动目录

### 4.5 监视

- fs.watchFile(filename[, options], listener)
- fs.watch(filename[, options][, listener])

### 4.6 文件流

## 5. HTTP

- 太简单了

## 扩展1：Github Pages

Github Pages

## 扩展2：静态博客生成器

https://hexo.io/zh-cn/

### 自动化部署

- 配置 _config.yml 文件中的 deploy 属性，指定 type 和 repo
- 在博客根目录下执行：`npm install hexo-deployer-git --save`
- [如果配置了SSH，可以省略这一步]
  + 如果没有配置，那就在刚才的 repo 指定的地址中加入用户名和密码
- 部署命令：`hexo deploy --generate`

## 目标

1. 能概述什么是文件模块、核心模块、第三方模块
2. 能掌握使用 npm 本地安装和全局安装的区别
3. 能掌握 path 路径操作模块的基本使用
4. 能解释文件操作中同步调用和异步调用的区别
5. 能熟练掌握Promise的用法并灵活使用
