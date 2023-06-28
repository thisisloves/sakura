---
title: React-App配置
date: 2019-1-7
tags: React
author: 映雪
---

得不到的总是挂念，共朝夕的总是厌倦。

<!--more-->

### react-app 创建

```
npm install -g create-react-app  // 安装
create-react-app my-app          // 创建
```

### react-app 结构

[![1.png](/images/2019/06/29/5d1716db9282361182.png)](/images/2019/06/29/5d1716db9282361182.png)


### 抽离配置文件

- 默认react 的项目的配置文件包含在 react-scripts 模块内部
- 可以运行 `npm run eject` 抽离配置文件

[![3.png](/images/2019/06/29/5d1716d9c3f9766778.png)](/images/2019/06/29/5d1716d9c3f9766778.png)

- 注意: 报错需先运行以下命令

```
git add .
git commit -m "init"
```


### 配置相对路径

- 配置 src 文件夹的别名 @

[![6.png](/images/2019/06/29/5d1716ddac08d44574.png)](/images/2019/06/29/5d1716ddac08d44574.png)


### 配置 less

```
npm install less less-loader --save

```

- 新增less 配置变量

```js
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
const lessRegex = /\.less$/;  // 新增less配置
const lessModuleRegex = /\.module\.less$/; // 新增less配置
```

- 增加module下面rule规则

```js
{
    test: lessRegex,
    exclude: lessModuleRegex,
    use: getStyleLoaders({
            importLoaders: 1,// 值是1
            modules: true, // 增加这个可以通过模块方式来访问css
            sourceMap: isEnvProduction && shouldUseSourceMap
        },
        "less-loader"
    ),
    sideEffects: true
}, 
{
    test: lessModuleRegex,
    use: getStyleLoaders({
            importLoaders: 1,
            sourceMap: isEnvProduction && shouldUseSourceMap,
            modules: true,
            getLocalIdent: getCSSModuleLocalIdent
        },
        "less-loader"
    )
},
// "file" loader makes sure those assets get served by WebpackDevServer.
```


### 路由配置

```
npm i react-router-dom -S
```

- 入口文件处引入相关文件并且配置

```js
import React from 'react';
import './index.css';
import reportWebVitals from './reportWebVitals';
import { createRoot } from 'react-dom/client';
import { HashRouter, Route, Switch, Redirect, Link } from 'react-router-dom';
import TableComponent from '@/pages/Table/index';
import HomeComponent from '@/pages/Home/index';
import { Button } from 'antd';

const container = document.getElementById('root');
const root = createRoot(container);
root.render(
  <HashRouter>
    <div className="container">
      <div className="menu">
        <Button>
          <Link to="/home">home</Link>
        </Button>
        <Button>
          <Link to="/table">table</Link>
        </Button>
      </div>
      <Switch>
        <Route path="/home" component={HomeComponent}></Route>
        <Route path="/table" component={TableComponent}></Route>
        <Redirect to="/home" />
      </Switch>
    </div>
  </HashRouter>,
);

reportWebVitals();

```