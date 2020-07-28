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

## 代码分离（entry手动分离、SplitChunksPlugin 去重和分离 chunk、通过内联函数调用分离代码）
1. 入口起点(entry point)
2. 防止重复(SplitChunksPlugin、mini-css-extract-plugin)
3. 动态导入
4. 预获取/预加载模块
5. bundle 分析