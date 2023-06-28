---
title: Webpack
date: 2020-12-15
tags: 前端
author: 映雪
---

圣人不法古，不脩今。法古则后于时，脩今则塞于势。

<!--more-->

### Webpack

- Webpack 是代码编译工具，有入口、出口、loader 和插件。webpack 是一个用于现代 JavaScript 应用程序的静态模块打包工具。当 webpack 处理应用程序时，它会在内部构建一个依赖图(dependency graph)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 bundle。

![截屏2022-06-14 下午4.02.58.png](/images/2022/06/14/QS8RFJG6mcb3xuf.png)

### Webpack 作用

1. 代码压缩

- 将 JS、CSS 代码混淆压缩，让代码体积更小，加载更快

2. 编译语法

- 编写 CSS 时使用 Less、Sass，编写 JS 时使用 ES6、TypeScript 等，这些标准目前都无法被浏览器兼容，因此需要构建工具编译，例如使用 Babel 编译 ES6 语法。

3. 处理模块化：

- CSS 和 JS 的模块化语法，目前都无法被浏览器兼容。因此开发环境可以使用既定的模块化语法，但是需要构建工具将模块化语法编译为浏览器可识别形式。例如使用 webpack、Rollup 等处理 JS 模块化。

### Webpack 安装

```
npm install --save-dev webpack webpack-cli
```

### Webpack 入口

- 入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

- webpack.config.js

```js
module.exports = {
  entry: './path/to/my/entry/file.js',
};
```

### Webpack 出口

- output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。你可以通过在配置中指定一个 output 字段，来配置这些处理过程。

- webpack.config.js

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
};
```

- 通过 output.filename 和 output.path 属性，来告诉 webpack bundle 的名称，以及想要 bundle 生成(emit)到哪里。

### loader

- loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

- 本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

- 在更高层面，在 webpack 的配置中 loader 有两个目标：

1. test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件。
2. use 属性，表示进行转换时，应该使用哪个 loader。

- webpack.config.js

```js
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};

module.exports = config;
```

### Plugins

- loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

- 想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。

- webpack.config.js

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

const config = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};

module.exports = config;
```
