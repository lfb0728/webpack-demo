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

## 构建性能
1. 通用环境
    - webpack node npm包管理工具 升级到最新版本提高构建性能
    - 将 loader 应用于最少数量的必要模块，通过使用 include 字段，仅将 loader 应用在实际需要将其转换的模块
    - 每个额外的 loader/plugin 都有其启动时间。尽量少地使用工具
    - 减少 resolve.modules, resolve.extensions, resolve.mainFiles, resolve.descriptionFiles 中条目数量，因为他们会增加文件系统调用的次数。
    - 如果不使用 symlinks（例如 npm link 或者 yarn link），可以设置 resolve.symlinks: false。
    - 如果使用自定义 resolve plugin 规则，并且没有指定 context 上下文，可以设置 resolve.cacheWithContext: false。
    - 使用 DllPlugin 为更改不频繁的代码生成单独的编译结果。这可以提高应用程序的编译速度，尽管它增加了构建过程的复杂度。
    - 使用数量更少/体积更小的 library。
    - 在多页面应用程序中使用 SplitChunksPlugin，并开启 async 模式。
    - 移除未引用代码。
    - 只编译你当前正在开发的那些代码。
    - thread-loader 可以将非常消耗资源的 loader 分流给一个 worker pool。
    - 使用 cache-loader 启用持久化缓存。使用 package.json 中的 "postinstall" 清除缓存目录。
    - 将 ProgressPlugin 从 webpack 中删除，可以缩短构建时间。请注意，ProgressPlugin 可能不会为快速构建提供太多价值，因此，请权衡利弊再使用。
2. 开发环境
    - 使用 webpack 的 watch mode(监听模式)。而不使用其他工具来 watch 文件和调用 webpack 。内置的 watch mode 会记录时间戳并将此信息传递给 compilation 以使缓存失效。
    - 在某些配置环境中，watch mode 会回退到 poll mode(轮询模式)。监听许多文件会导致 CPU 大量负载。在这些情况下，可以使用 watchOptions.poll 来增加轮询的间隔   时间。
    - 在内存中编译 （webpack-dev-server、webpack-hot-middleware、webpack-dev-middleware）
    - 需要注意的是不同的 devtool 设置，会导致性能差异。"eval" 具有最好的性能，但并不能帮助你转译代码。 如果你能接受稍差一些的 map 质量，可以使用 cheap-source-map 变体配置来提高性能。 使用 eval-source-map 变体配置进行增量编译。 在大多数情况下，最佳选择是 eval-cheap-module-source-map。
    - 避免在生产环境下才会用到的工具 （TerserPlugin、ExtractTextPlugin、[hash]/[chunkhash]、AggressiveSplittingPlugin、AggressiveMergingPlugin、ModuleConcatenationPlugin）
    - 最小化 entry chunk
    - 避免额外的优化步骤（optimization） webpack 通过执行额外的算法任务，来优化输出结果的体积和加载性能。这些优化适用于小型代码库，但是在大型代码库中却非常耗费性能
    - 输出结果不携带路径信息  options.output.pathinfo 设置中关闭。
    - 你可以为 loader 传入 transpileOnly 选项，以缩短使用 ts-loader 时的构建时间。使用此选项，会关闭类型检查。如果要再次开启类型检查，请使用 ForkTsCheckerWebpackPlugin。使用此插件会将检查过程移至单独的进程，可以加快 TypeScript 的类型检查和 ESLint 插入的速度。
3. 生产环境
    - 在创建多个 compilation 时，工具可以帮助到你（parallel-webpack：它允许在一个 worker 池中运行 compilation。cache-loader：可以在多个 compilation 之间共享缓存）
    - 关闭Source Maps
    - Babel 最小化项目中的 preset/plugin 数量。
    - TypeScript 在单独的进程中使用 fork-ts-checker-webpack-plugin 进行类型检查。 配置 loader 跳过类型检查。使用 ts-loader 时，设置 happyPackMode: true / transpileOnly: true。
    - Sass node-sass 中有个来自 Node.js 线程池的阻塞线程的 bug。 当使用 thread-loader 时，需要设置 workerParallelJobs: 2。

## 模块热替换
1. 启用 HMR
2. 通过 Node.js API
3. HMR 加载样式