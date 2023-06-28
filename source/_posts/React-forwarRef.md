---
title: React.forwarRef
date: 2020-6-16
tags: React
author: 映雪
---

楚天千里清秋，水随天去秋无际。

<!--more-->

### React.forwardRef 

- React.forwardRef 会创建一个React组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中。
- React.forwardRef 接受渲染函数作为参数。React 将使用 props 和 ref 作为参数来调用此函数。此函数应返回 React 节点。

### React.forwardRef 基本使用

```jsx
const  A = React.forwardRef((props,ref)=>(
<div ref={ref}>
<B {...props} />
</div>
))

//创建一个React组件
//这个组件将会接受到父级传递的ref属性
//可以将父组件创建的ref挂到子组件的某个dom元素上
//在父组件通过该ref就能获取到该dom元素
```

- React.forwardRef 返回的是一个组件
- React.forwardRef 接受一个函数作为参数，这个函数是一个渲染函数，也就是一个组件
- 该渲染函数接受 props 和 ref 作为参数

### forwardRef与useRef 对比

```jsx
import { Button,Form,Input } from 'antd';
import React, { useState, useRef, useEffect} from 'react';

const RendnerChild = (props)=>{
    const {cRef} = props;
    const [form] = Form.useForm();
    const [value, setValue] = useState("");

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
      <Form form={form} >
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
        alert(node.getValue())
    }

    return <>
    <RendnerChild cRef={childRef}/>
    <Button onClick={()=>{handleAlertClick()}}>show</Button>
    </>
}

export default App;
```

- 使用forwarRef

```jsx
import { Button,Form,Input } from 'antd';
import React, { useState, useRef, useEffect,forwardRef} from 'react';

const RendnerChild = forwardRef((props,ref)=>{
    const [form] = Form.useForm();
    const [value, setValue] = useState("");

    const handleChange = (e) => {
        const value = e.target.value
        setValue(value)
    }

    return   <>
      <Form  form={form}>
        <Form.Item props="name" label="名字">
            <Input ref={ref} type='text' value={value} onChange={handleChange}></Input>
        </Form.Item>
      </Form>
    </>
})

const App = () => {
    const childRef = useRef({})

    const handleAlertClick = ()=>{ 
        const node = childRef.current
        console.log(node.input.value)
        alert(node.input.value)
    }

    return <>
      <RendnerChild ref={childRef}/>
      <Button onClick={()=>{handleAlertClick()}}>show</Button>
    </>
}

export default App;
```

- forwardRef 在参数函数中向其传入ref参数，ref附着谁，就被外界ref.current 所取得，forwardRef实现的功能就是外层组件可以通过 ref 直接控制内层组件或元素的行为，简单的来说，就像其名字：透传。
- **forwardRef的API中ref必须指向dom元素。**