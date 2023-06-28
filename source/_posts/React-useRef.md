---
title: useRef
date: 2020-6-15
tags: React
author: 映雪
---

舞榭歌台，风流总被，雨打风吹去。

<!--more-->

### refs(类组件)

- refs 提供了一种方式，允许我们访问DOM节点或在render方法中创建的React元素。
- 在类组件中使用时，可以通过 createRef 来使用，每次渲染都会创建一个新的 ref 对象。


### useRef(函数组件)

- useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 可变对象在组件的整个生命周期内持续存在且不变。
- 更新current 值不会render，和useState的区别。


### useRef 使用

- **跨渲染获取状态**

```jsx
import { Button, Modal } from 'antd';
import React, { useState, } from 'react';

const RendnerChild = (props)=>{
    const c = props.count
    return   <Modal visible={props.visible}> <p>{c}</p></Modal>
}

const App = () => {
    const [count, setCount] = useState(0);
    const [visible,setVisible] = useState(false)

    const handleAlertClick = ()=>{  // 无法共享变化的state 值
        setTimeout(()=>{
            alert (`count:${count}`)
        },3000)
    }

    return <>
        <p>count:{count}</p>
        <Button onClick={(() => setCount((count)=>count+1))}>添加+1</Button>
        <Button onClick={(() => setCount((count)=>count-1))}>减少-1</Button>
        <Button onClick={()=>{setVisible(true);setTimeout(()=>setCount((count)=>count+10),1000)}}>show Modal</Button>
        <RendnerChild count={count} visible={visible}></RendnerChild>

        <Button onClick={()=>{handleAlertClick()}}>show</Button>

    </>
}
```

- 更改状态的时候，React会重新渲染组件，每次的渲染都会拿到独立的count ，而点击触发alert 只能取当时的值，后面count 变化无法取得。

1. 解决方案1 - **使用全局变量**

```jsx
import { Button, Modal } from 'antd';
import React, { useState, } from 'react';

let like=0 

const handleAlertClick2 = ()=>{   // 内存中like变化，取值为内存中的like
    setTimeout(()=>{
        alert (`like:${like}`)
    },3000)
}

const App = () => {
    const [count, setCount] = useState(0);

    const handleAlertClick = ()=>{  // 无法共享变化的state 值
        setTimeout(()=>{
            alert (`count:${count}`)
        },3000)
    }

    return <>
        <p>count:{count}</p>
        <Button onClick={(() => setCount((count)=>count+1))}>添加+1</Button>
        <Button onClick={(() => setCount((count)=>count-1))}>减少-1</Button>
        <Button onClick={()=>{handleAlertClick()}}>show</Button>
        <br/>

        <p>like:{like}</p>  {/*  一直是0 ，数据无法驱动view变化 */}
        <Button onClick={(() => {like= ++like} )}>添加+1</Button> {/* 每次点击实际like 在递增 */}
        <Button onClick={()=>{handleAlertClick2()}}>show</Button>
    </>
}

export default App;
```

1. 解决方案2 - **使用useRef**

```jsx
import { Button, Modal } from 'antd';
import React, { useState, useRef} from 'react';

const App = () => {
    const [count, setCount] = useState(0);

    const handleAlertClick = ()=>{  // 无法共享变化的state 值
        setTimeout(()=>{
            alert (`count:${count}`)
        },3000)
    }

    let like= useRef(0) 

    const handleAlertClick2 = ()=>{   // 内存中like变化，取值为内存中的like
        setTimeout(()=>{
            alert (`like:${like.current}`)
        },3000)
    }
    
    return <>
        <p>count:{count}</p>
        <Button onClick={(() => setCount((count)=>count+1))}>添加+1</Button>
        <Button onClick={(() => setCount((count)=>count-1))}>减少-1</Button>
        <Button onClick={()=>{handleAlertClick()}}>show</Button>
        <br/>
        <p>like:{like.current}</p>  {/*  一直是0 ，数据无法驱动view变化 */}
        <Button onClick={(() => {like.current= ++like.current} )}>添加+1</Button> {/* 每次点击实际like 在递增 */}
        <Button onClick={()=>{handleAlertClick2()}}>show</Button>
    </>
}

```
### useRef 和 全局变量的区别

- 使用全局变量也能实现 useRef 的功能

1. seRef 是定义在实例基础上的，如果代码中有多个相同的组件，每个组件的 ref 只跟组件本身有关，跟其他组件的 ref 没有关系。
2. 组件前定义的 global 变量，是属于全局的。如果代码中有多个相同的组件，那这个 global 变量在全局是同一个，他们会互相影响


### useRef 和 createRef 的区别

- 组件生命周期

1. 从创建组件到挂载到DOM阶段。初始化props以及state, 根据state与props来构建DOM。
2. 组件依赖的props以及state状态发生变更，触发更新。
3. 销毁阶段。

- 区别

1. 第一个阶段，useRef与createRef没有差别
2. 第二个阶段，createRef每次都会返回个新的引用;而useRef不会随着组件的更新而重新创建
3. 第三个阶段，两者都会销毁


### 获取子组件的属性和方法

```jsx
import { Button,Form,Input } from 'antd';
import React, { useState, useRef, useEffect} from 'react';


const RendnerChild = (props)=>{
    const {cRef} = props;
    const [form] = Form.useForm();
    const [value, setValue] = useState("");

    const onFinish = (values) => {
        console.log(values);
      };
      const getValue = () => {
        return value
    }
      const handleChange = (e) => {
        const value = e.target.value
        setValue(value)
    }


    useEffect(()=>{
      if(cRef&& cRef.current){
        cRef.current.getValue = getValue
      }
    },[value])

    return   <>
      <Form  form={form}  onFinish={onFinish}>
        <Form.Item props="name" label="名字">
            <Input type='text' value={value} onChange={handleChange}></Input>
        </Form.Item>
      </Form>
    </>
}

const App = () => {
    const childRef = useRef({})

    const handleAlertClick = ()=>{ 
        const node = childRef.current
        console.log(node)
        alert(node.getValue())
    }

    return <>
    <RendnerChild cRef={childRef}/>
        <Button onClick={()=>{handleAlertClick()}}>show</Button>
    </>
}

export default App;
```