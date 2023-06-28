---
title: JS条件语句
date: 2019-1-03
tags: JS
author: 映雪
---

枕上一书三生誓，殷切与君语迟迟。 佛铃花落醉尘火，执念难绝情成痴。

<!--more-->

### 条件语句

- 基于不同的条件来执行不同的动作。

### if语句

- 只有当指定条件为 true 时，该语句才会执行代码。

```js
if (time<20)
{
   console.log('我执行了')
}
```

### if...else 语句

- 在条件为 true 时执行代码，在条件为 false 时执行其他代码。

```js
if (time<20)
{
 console.log('true,我执行了')
}
else
{
 console.log('false,我执行了')
}
```


### if...else if...else 语句

- 选择多个代码块之一来执行。 

```js
if (time<10)
{
 console.log('time<10,我执行了')
}
else if (time>=10 && time<20)
{
 console.log('20>tim>10,我执行了')
}
else
{
 console.log('我执行了')
}
```

### switch 语句

- 基于不同的条件来执行不同的动作。

```js
var d=new Date().getDay(); 
switch (d) 
{ 
  case 0:x="今天是星期日"; 
  break; 
  case 1:x="今天是星期一"; 
  break; 
  case 2:x="今天是星期二"; 
  break; 
  case 3:x="今天是星期三"; 
  break; 
  case 4:x="今天是星期四"; 
  break; 
  case 5:x="今天是星期五"; 
  break; 
  case 6:x="今天是星期六"; 
  break; 
  default:
    x="期待周末";
}

```

- switch 表达式的值会与结构中的每个 case 的值做比较。如果存在匹配，则与该 case 关联的代码块会被执行。
- break 会阻止代码自动地向下一个 case 运行。
