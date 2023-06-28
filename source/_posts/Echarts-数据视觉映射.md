---
title: Echarts数据视觉映射
date: 2018-10-5
tags: Echarts
author: 映雪
---

人生自是有情痴，此恨不关风与月。

<!--more-->

### Echarts 数据视觉映射

- 数据可视化简单来讲就是将数据用图表的形式来展示，专业的表达方式就是数据到视觉元素的映射过程。
- ECharts 的每种图表本身就内置了这种映射过程。
- 此外，ECharts 还提供了 visualMap 组件 来提供通用的视觉映射。visualMap 组件中可以使用的视觉元素有：

1. 图形类别（symbol）
2. 图形大小（symbolSize）
3. 颜色（color）
4. 透明度（opacity）
5. 颜色透明度（colorAlpha）
6. 颜色明暗度（colorLightness）
7. 颜色饱和度（colorSaturation）
8. 色调（colorHue）

### 数据和维度

- ECharts 中的数据，一般存放于 series.data 中。
- 不同的图表类型，数据格式有所不一样，但是他们的共同特点就都是数据项（dataItem） 的集合。每个数据项含有 数据值（value） 和其他信息（可选）。每个数据值，可以是单一的数值（一维）或者一个数组（多维）。
- series.data 最常见的形式 是线性表，即一个普通数组：

```js
series: {
    data: [
        {       // 这里每一个项就是数据项（dataItem）
            value: 2323, // 这是数据项的数据值（value）
            itemStyle: {...}
        },
        1212,   // 也可以直接是 dataItem 的 value，这更常见。
        2323,   // 每个 value 都是『一维』的。
        4343,
        3434
    ]
}
series: {
    data: [
        {                        // 这里每一个项就是数据项（dataItem）
            value: [3434, 129,  '圣马力诺'], // 这是数据项的数据值（value）
            itemStyle: {...}
        },
        [1212, 5454, '梵蒂冈'],   // 也可以直接是 dataItem 的 value，这更常见。
        [2323, 3223, '瑙鲁'],     // 每个 value 都是『三维』的，每列是一个维度。
        [4343, 23,   '图瓦卢']    // 假如是『气泡图』，常见第一维度映射到x轴，
                                 // 第二维度映射到y轴，
                                 // 第三维度映射到气泡半径（symbolSize）
    ]
}
```

### visualMap 组件

- visualMap 组件定义了把数据的指定维度映射到对应的视觉元素上。
- visualMap 组件可以定义多个，从而可以同时对数据中的多个维度进行视觉映射。
- visualMap 组件可以定义为 分段型（visualMapPiecewise） 或 连续型（visualMapContinuous），通过 type 来区分。例如：

```js
option = {
    visualMap: [
        { // 第一个 visualMap 组件
            type: 'continuous', // 定义为连续型 visualMap
            ...
        },
        { // 第二个 visualMap 组件
            type: 'piecewise', // 定义为分段型 visualMap
            ...
        }
    ],
    ...
};
```

- 分段型视觉映射组件，有三种模式：

1. 连续型数据平均分段: 依据 visualMap-piecewise.splitNumber 来自动平均分割成若干块。
2. 连续型数据自定义分段: 依据 visualMap-piecewise.pieces 来定义每块范围。
3. 离散数据根据类别分段: 类别定义在 visualMap-piecewise.categories 中。
4. 分段型视觉映射组件，展现形式如下图


![截屏2022-05-13 下午2.43.36.png](/images/2022/05/13/EhA4SDPQvTRe3mX.png)

### 视觉映射方式的配置

- visualMap 中可以指定数据的指定维度映射到对应的视觉元素上。

```js
option = {
    visualMap: [
        {
            type: 'piecewise'
            min: 0,
            max: 5000,
            dimension: 3,       // series.data 的第四个维度（即 value[3]）被映射
            seriesIndex: 4,     // 对第四个系列进行映射。
            inRange: {          // 选中范围中的视觉配置
                color: ['blue', '#121122', 'red'], // 定义了图形颜色映射的颜色列表，
                                                    // 数据最小值映射到'blue'上，
                                                    // 最大值映射到'red'上，
                                                    // 其余自动线性计算。
                symbolSize: [30, 100]               // 定义了图形尺寸的映射范围，
                                                    // 数据最小值映射到30上，
                                                    // 最大值映射到100上，
                                                    // 其余自动线性计算。
            },
            outOfRange: {       // 选中范围外的视觉配置
                symbolSize: [30, 100]
            }
        },
        ...
    ]
};
```
