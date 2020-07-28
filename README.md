# webpack-demo

## 起步
1. 基本安装
2. bundle创建
3. 使用配置文件
4. 添加npm脚本

## 管理资源（在管理输出会回退至最初的版本）
1. 加载CSS(style-loader css-loader)
2. 加载images图像(file-loader)压缩和优化你的图像(image-webpack-loader url-loader)
3. 加载fonts字体(file-loader)
4. 加载数据(如 JSON 文件，CSV、TSV 和 XML)(csv-loader xml-loader)

## 管理输出
1. 设置 HtmlWebpackPlugin(html-webpack-plugin)
2. 清理/dist文件夹(clean-webpack-plugin)

## 开发环境
1. 设置mode 为 development
2. 使用source map (inline-source-map)
3. 使用watch mode(观察者模式) 添加一个用于启动 webpack watch mode 的 npm scripts: watch 缺点：改正后需要刷新浏览器
4. 使用webpack-dev-server(webpack-dev-server) 添加一个用于启动 webpack-dev-server 的 npm scripts: start
5. 使用webpack-dev-middleware(express webpack-dev-middleware) 添加一个server.js文件配置，再添加一个用于启动 webpack-dev-middleware 的 npm scripts: server

## 代码分离（entry手动分离、SplitChunksPlugin 去重和分离 chunk、通过内联函数调用分离代码）（需要深入）
1. 入口起点(entry point)
2. 防止重复(SplitChunksPlugin、mini-css-extract-plugin) SplitChunksPlugin 可以用于将模块分离到单独的 bundle 中
3. 动态导入
4. 预获取/预加载模块
5. bundle 分析

## 缓存（需要深入）
1. 输出文件的文件名 [contenthash] 对比变化: 文件没有修改，再次构建文件名会保持不变
2. 提取引导模板 可以通过SplitChunkPlugin插件的cacheGroups来将第三方库(library)（例如 lodash 或 react）提取到单独的 vendor chunk 文件中
3. 模块标识符 optimization.moduleIds 设置为 'hashed'：第三方vendor的hash都保持一致

## 创建 library（需要深入）
1. 基本配置
    - 使用 externals 选项，避免将 lodash 打包到应用程序，而使用者会去加载它。
    - 将 library 的名称设置为 webpack-numbers。
    - 将 library 暴露为一个名为 webpackNumbers 的变量。
    - 能够访问其他 Node.js 中的 library。
    - ES2015 模块。例如 import webpackNumbers from 'webpack-numbers'。
    - CommonJS 模块。例如 require('webpack-numbers')。
    - 全局变量，在通过 script 标签引入时。
2. 使用 source map 的基本配置
3. 外部化 lodash 将一些第三库如 lodash 当作 peerDependency 依赖，可以通过 externals 配置来完成
4. 外部化的限制 对于想要实现从一个依赖中调用多个文件的那些 library， 使用正则表达式来从bundle中排除
5. 暴露 library  在output中添加 library: 'webpackNumbers', 为了让 library 和其他环境兼容，则需要在配置中添加 libraryTarget 属性。

## 环境变量（想要消除 webpack.config.js 在 开发环境 和 生产环境 之间的差异，需要环境变量）
对于 webpack 配置，有一个必须要修改之处。通常，module.exports 指向配置对象。要使用 env 变量，你必须将 module.exports 转换成一个函数