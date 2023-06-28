---
title: 防抖和节流
date: 2019-4-1
tags: JS
author: 映雪
---

适度的温婉就是温柔，温柔不是示弱，不是妥协，只是真诚，善良，在安静中不慌不忙的坚强。

<!--more-->

### 防抖（debounce）

- 防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
- 应用情景：主要用于交互层，最常见的是动态搜索框，如果不做防抖处理会不断的向服务器去查询数据，而最好的解决办法是让用户停止输入的时候再去进行查询搜索。

```html
<html>
  <body>
    <div>
      <label>输入：</label>
      <input type="text" id="input" />
    </div>
  </body>
  <script>
    let ele = document.getElementById('input');

    function debounce(fun, delay) {
      return function (args) {
        let that = this;
        let _args = args;
        clearTimeout(fun.id);
        fun.id = setTimeout(function () {
          fun.call(that, _args);
        }, delay);
      };
    }
    var fun = debounce((e) => {
      console.log('输入框值为：', e);
    }, 500);

    ele.addEventListener('keyup', function (e) {
      fun(e.target.value);
    });
  </script>
</html>
```

### 节流（throttle）

- 函数节流指的是在一定的单位时间内，只能触发一次函数，如果一定时间内触发多次，只有一次生效。
- 应用场景：监听滚动事件，是否滑到底部自动加载等

```html
<html>
  <body>
    <div>
      <label>输入：</label>
      <input type="text" id="input" />
    </div>
  </body>
<script>

let ele = document.getElementById('input');

var throttle = function (func, wait) {
    var lastTime = 0 // 用来记录上次执行的时刻
    // 返回包装过的throttle函数
    return function (...args) {
        var now = Date.now()
        var coolingDown = now - lastTime < wait;
        if (coolingDown) {
            return
        }
        // 记录本次执行的时刻
        lastTime = Date.now()
        func.apply(null, args)
    }
}

var fun = throttle((e)=>{console.log('输入框值为：',e);},2000);

ele.addEventListener('keyup', function (e) {
    fun(e.target.value)
})
  </script>
</html>
```

### 区别

- 相同点

1. debounce 防抖与 throttle 节流都实现了单位时间内，函数只执行一次

- 不同点

1. debounce 防抖：一定时间内，前面反复触发的事件，只会响应最新的，并在一定的秒数后执行。
2. throttle 节流：响应第一次的，单位时间内，不再响应，直到一定的秒数后才再次响应。
