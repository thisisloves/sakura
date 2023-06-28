---
title: Echarts自适应(rem)
date: 2019-1-5
tags: Echarts
author: 映雪
---

掬水月在手，弄花香满衣。

<!--more-->

### rem 

- 通过计算html根元素的font-size，获得 rem 的动态基准值

### rem 优缺点

1. 布局的自适应代码量少，适配简单
2. 因为是根据 ui 稿等比缩放，当大屏跟 ui 稿的比例不一样时，要单个做字体、间距、位移的适配

### Echarts自适应

- echarts多个图表大小随屏幕的大小改变自适应，Echarts 多图表自适应窗口大小，echarts内容随页面大小变化而变化。

### Echarts图表随屏幕的大小改变

- 使用百分比布局，当屏幕大小变化，div块会自动变化。

### Echarts图表外内容自适应

- 在echarts外的字体距离适应多个屏幕。

1. 下载第三方计算js库

```js
pnpm add lib-flexible-computer postcss-px2rem px2rem-loader -D
```

2. 在main.ts里引入

```js
import "lib-flexible-computer";
```

3. 在vite.config.ts写入

```js
import px2rem from "postcss-px2rem"
 
 css: {
      postcss: {
          plugins: [
            px2rem({
              remUnit:192
            })
          ]
      }
  }
```

### Echarts图表内容大小自适应

- 在echarts里面不能使用vw、rem这样的单位，只能使用px，因此echarts中字体边距等涉及到尺寸的数值都要进行换算。

```jsx
/* Echarts图表字体、间距等自适应 */
export function fontSize  (size: any){
  let clientWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
  if (!clientWidth) return size;
  let scale =  (clientWidth / 1920);
  return size * scale;
}

// 使用
splitLine: {
    show: true,
    length: -fontSize(10),
    lineStyle: {
    color: '#fff',
    width: fontSize(1)
    }
},
axisLabel: {
    distance: -fontSize(15),
    textStyle: {
    color: '#fff',
    fontSize: fontSize(10),
    }
}
```

### Echarts图表自适应窗口

- echarts可视化图表，需要echarts会随着浏览器的放大，缩小自适应div的大小。

```jsx
let chart: ECharts;
const chartRef: Ref<HTMLElement | null> = ref(null)
let ele:any = null
const resizeObserver = new ResizeObserver(_entries => {
    powerSupplyChart(chart, props.dayElectric,props.surplusElectric)
});


onMounted(() => {
    chart = init(chartRef.value as HTMLElement)
    powerSupplyChart(chart, props.dayElectric,props.surplusElectric)
    ele = chartRef.value
    resizeObserver.observe(ele)
})

onBeforeUnmount(() => {
    ele = null
})

// 渲染图表方法
export const powerSupplyChart = (echarts: any, dayElectric: string | number | undefined,surplusElectric: string | number | undefined) => {
    var option =  {
      grid: {
        left: '1%',
        right: '4%',
        bottom: '1%',
        top:'1%',
        containLabel: true
      },
      xAxis: {
        min: 0,
        max: 1000,
        show: false,
        type: "value",
      },
      yAxis: {
        min: 0,
        max: 1000,
        show: false,
        type: "value",
      },
      series: [
        {
          type: "graph",
          coordinateSystem: "cartesian2d",
          hoverAnimation: false,
          label: {
            show: true,
            color: "#fff",
          },
          data: charts.nodes,
        },
       
      ],
    };;
    option && echarts.setOption(option)
    echarts.resize()
}

```

