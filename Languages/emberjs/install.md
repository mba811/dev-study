# 安装 emberjs

安装emberjs很简单! 我们引入了一个新的安装方式emberj cli, Ember's 构建工具

  1. 静态资源的管理（合并，压缩和管理[combining, minifying, and versioning]）

  2. 内置的生长工具可以帮助你创建组件，路由和更多（和他们的测试案例。）

  3. 一个标准的工程布局. 工作在emberjs很简单, 他们是有类似的条理

  4. 用原生的Javascript模块来保持项目的条理性

  5. 一个完整的测试框架 (单元测试, 集成测试)

  6. 访问一个不断增长的生态系统的emberjs插件. 你的应用程序中添加功能而无需编写一行代码! 他们已经打包准备加入到你的应用程序

## 安装

用npm安装emberjs. 我们推荐你同时安装phantomjs（如果你还没有安装）. Ember CLI 用 phantomjs 用于命令行运行测试 (不需要你在浏览器运行测试).
    
    npm install -g ember-cli
    npm install -g phantomjs
    

## 测试你的安装
    
    ember new my-app
    

这里将为你创建一个新的my-app文件夹，并生成一个web应用结构.

一旦构建成功, 我们来验证我们生成的新应用：
    
    cd my-app
    ember server
    

打开浏览器运行 [http://localhost:4200](http://localhost:4200/) 看看你的第一个emberjs

## 故障排除

### GOT NODE (AND NPM)?

Node 安装包管理工具 (npm) 捆绑于 node.js 和让安装更简单. 如果你不确定node.js已经安装, 试试在命令行运行:
    
    node --version
    

如果你看到类似的输出表示你的node安装成功了
    
    C:\Users\QKL>node --version
    v0.10.15
    

如果你还没有安装....

  * [Windows or Mac 用户可以的下载直接安装](http://nodejs.org/download/).

  * Linux 用户可以自己百度nodejs安装方法.
