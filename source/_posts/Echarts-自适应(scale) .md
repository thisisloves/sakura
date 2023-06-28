---
title: Echarts自适应(scale)
date: 2019-1-5
tags: Echarts
author: 映雪
---

曲终人不见，江上数峰青。

<!--more-->

### scale 

- 通过 scale 属性，根据屏幕大小，对图表进行整体的等比缩放。

### scale 优缺点

1. 代码量少，适配简单 ，一次处理后不需要在各个图表中再去单独适配。
2. 因为是根据 ui 稿等比缩放，当大屏跟 ui 稿的比例不一样时，会出现周边留白情况
3. 当缩放比例过大时候，字体会有一点点模糊
4. 当缩放比例过大时候，事件热区会偏移
5. **涉及百度地图等地图放大缩小会出现问题**

![截屏2022-01-18 上午10.40.55.png](/images/2023/06/27/windows.png)


- 和设计图比例不一致出现留白

### Echarts自适应

- echarts多个图表大小随屏幕的大小改变自适应，Echarts 多图表自适应窗口大小，echarts内容随页面大小变化而变化。


### scale 使用

```jsx
import { Button, Form, Input } from 'antd';
import React, { useEffect } from 'react';
import styles from './index.less';
import Box from '../../components/Box'

const Scale = () => {
  //数据大屏自适应函数
  const handleScreenAuto = () => {
    const designDraftWidth = 1920; //设计稿的宽度
    const designDraftHeight = 1080; //设计稿的高度
    //根据屏幕的变化适配的比例
    const scale =
      document.documentElement.clientWidth / document.documentElement.clientHeight < designDraftWidth / designDraftHeight
        ? document.documentElement.clientWidth / designDraftWidth
        : document.documentElement.clientHeight / designDraftHeight;
    //缩放比例
    document.querySelector("#screen").style.transform = `scale(${scale}) translate(-50%)`;
  };


  useEffect(() => {
    //初始化自适应  --在刚显示的时候就开始适配一次
    handleScreenAuto();
    //绑定自适应函数   ---防止浏览器栏变化后不再适配
    window.onresize = () => handleScreenAuto();
    //退出大屏后自适应消失 
    return () => (window.onresize = null);
  }, []);


  return <>
    <div className={styles.screenWrapper}>
      <div className={styles.screen} id="screen">
        <div className={styles.content} >
          <div className={styles.header}  >
            我是标题哦
          </div>
          <div className={styles.container}>
            <div className={styles.right}>
              <div className={styles.rightTop}>
                <Box>
                  <Button onClick={handleFullScreen}></Button>
                </Box>
              </div>
              <div className={styles.rightCenter}>

              </div>
              <div className={styles.rightBottom}>

              </div>
            </div>
            <div className={styles.center}>
              <div className={styles.centerTop}>

              </div>
              <div className={styles.centerBottom}>

              </div>
            </div>

            <div className={styles.left}>
              <div className={styles.leftTop}>

              </div>
              <div className={styles.leftBottom}>

              </div>
            </div>

          </div>
        </div>
      </div>
    </div>
  </>
}

export default Scale;
```

- css

```less
.screenWrapper {
    height: 100%;
    width: 100%;

    .screen {
        display: inline-block;
        width: 1920px; //设计稿的宽度
        height: 1080px; //设计稿的高度
        transform-origin: 0 0;
        position: absolute;
        left: 50%;

        .content {
            width: 1920px; //设计稿的宽度
            height: 1080px; //设计稿的高度
        }

        .header {
            height: 80px;
            width: 1920px;
            background: #ccc;
        }

        .container {
            height: 980px;
            width: 1900px;
            display: flex;
            padding: 10px;

            .left {
                width: 540px;
                height: 980px;

                .leftTop {
                    height: 485px;
                    width: 540px;
                    background: blanchedalmond;
                    margin-bottom: 10px;
                }

                .leftBottom {
                    height: 485px;

                    width: 540px;
                    background: blue;

                }

                .leftCenter {
                    height: 300px;
                    width: 540px;
                    background: blanchedalmond;


                }

            }

            .center {
                width: 800px;
                height: 980px;
                margin: 0 10px;

                .centerTop {
                    height: 600px;
                    width: 800px;
                    background: aqua;
                }

                .centerBottom {
                    margin-top: 10px;
                    height: 370px;
                    width: 800px;
                    background: yellow;
                }
            }

            .right {
                width: 540px;
                height: 980px;

                .rightTop {
                    height: 320px;
                    width: 540px;
                    background: blanchedalmond;
                }

                .rightCenter {
                    height: 320px;
                    width: 540px;
                    background: blue;

                    margin: 10px 0;
                }

                .rightBottom {
                    height: 320px;
                    width: 540px;
                    background: blanchedalmond;
                }
            }
        }
    }
}
```

### 封装库使用

```js
import { Button, Form, Input } from 'antd';
import React, { useEffect } from 'react';
import styles from './index.less';
import Box from '../../components/Box'
import FitScreen from '@fit-screen/react'
const Scale = () => {

  const handleFullScreen = (params) => {
    var element = document.documentElement;
    if (element.requestFullscreen) {
      element.requestFullscreen();
    } else if (element.webkitRequestFullScreen) {
      element.webkitRequestFullScreen();
    } else if (element.mozRequestFullScreen) {
      element.mozRequestFullScreen();
    } else if (element.msRequestFullscreen) {
      // IE11
      element.msRequestFullscreen();
    }
  };


  return <FitScreen width={1920} height={1080}>
    <div className={styles.screenWrapper}>
      <div className={styles.screen} id="screen">
        <div className={styles.content} >
          <div className={styles.header}  >
            我是标题哦
          </div>
          <div className={styles.container}>
            <div className={styles.right}>
              <div className={styles.rightTop}>
                <Box>
                  <Button onClick={handleFullScreen}></Button>
                </Box>
              </div>
              <div className={styles.rightCenter}>

              </div>
              <div className={styles.rightBottom}>

              </div>
            </div>
            <div className={styles.center}>
              <div className={styles.centerTop}>

              </div>
              <div className={styles.centerBottom}>

              </div>
            </div>

            <div className={styles.left}>
              <div className={styles.leftTop}>

              </div>
              <div className={styles.leftBottom}>

              </div>
            </div>

          </div>
        </div>
      </div>
    </div>
  </FitScreen>
}
```