---
title: React.lazy
date: 2020-6-17
tags: React
author: 映雪
---

江流天地外，山色有无中。

<!--more-->

### React.lazy

- lazy 能够让你在组件第一次被渲染之前延迟加载组件的代码。
- 延迟加载是一种仅在需要时加载 Web 应用程序部分的技术，而不是一次全部加载。这可以通过减少最初需要加载的代码量来提高应用程序的性能。

### 懒加载、预加载

1. 懒加载

- 在需要的时候再去加载资源。

2. 预加载 

- 提前加载好下一步使用的资源

### React.lazy 代码分割

- 进行代码分割能够帮助“懒加载”当前用户所需要的内容，能够显著地提高应用性能。尽管并没有减少应用整体的代码体积，但可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。

```jsx
import { Button, Modal } from 'antd';
import { useState ,lazy,Suspense} from 'react';

const ModalPay = lazy(() => import('./Modal'))
const App = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const showModal = () => {
    setIsModalOpen(true);
  };

  const handleCancel = () => {
    setIsModalOpen(false);
  };
  return (
    <>
      <Button type="primary" onClick={showModal}>
        Open Modal
      </Button>
      <Suspense fallback={<div>loading</div>}>
      <ModalPay visible={isModalOpen} handleCancel={handleCancel}/>
      </Suspense>
    </>
  );
};
export default App;


import { Modal } from 'antd';
const PayModal = (props) => {
	const { visible, handleCancel } = props

	return <Modal title="Pay Modal" open={visible} onCancel={handleCancel}>
		<p>支付弹窗</p>
	</Modal>
}

export default PayModal
```

- React.lazy 对比同步组件打包

![截屏2023-06-16 上午9.08.07.png](/images/2023/06/16/qycjfpOlamrZ7ke.png)
![截屏2023-06-16 上午9.08.00.png](/images/2023/06/16/rUvkzn2tFfZKEpu.png)


- Modal组件文件被拆分打包为一个新的包（bundle）文件，并且只会在该组件渲染时，才会请求文件。
- 每次Modal点击都会不会重新加载DOM。

### React.lazy 懒加载

- 在代码分割基础上，添加懒加载(点击Modal时才去请求分割的包文件，之前是组件加载时就请求文件)

```jsx
import { Button, Modal } from 'antd';
import { useState ,lazy,Suspense} from 'react';

const ModalPay = lazy(() => import('./Modal'))
const App = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const showModal = () => {
    setIsModalOpen(true);
  };

  const handleCancel = () => {
    setIsModalOpen(false);
  };
  return (
    <>
      <Button type="primary" onClick={showModal}>
        Open Modal
      </Button>
      <Suspense fallback={<div>loading</div>}>
      {isModalOpen &&<ModalPay visible={isModalOpen} handleCancel={handleCancel}/>}
      </Suspense>
    </>
  );
};
export default App;


import { Modal } from 'antd';
const PayModal = (props) => {
	const { visible, handleCancel } = props

	return <Modal title="Pay Modal" open={visible} onCancel={handleCancel}>
		<p>支付弹窗</p>
	</Modal>
}

export default PayModal
```

- 打开控制台，观察 Elements 和 Network， 打开Modal时，才请求代码分割文件。
- 缺陷：观察 Elements，每次Modal点击都会重新加载DOM。

```jsx
import { Button, Modal } from 'antd';
import { useState ,lazy,Suspense} from 'react';

const ModalPay = lazy(() => import('./Modal'))
const App = () => {
  const [isRender,serIsRener] = useState(false)
  const [isModalOpen, setIsModalOpen] = useState(false);
  const showModal = () => {
    setIsModalOpen(true);
    serIsRener(true)
  };

  const handleCancel = () => {
    setIsModalOpen(false);
  };
  return (
    <>
      <Button type="primary" onClick={showModal}>
        Open Modal
      </Button>
      <Suspense fallback={'loading'}>
      {isRender &&<ModalPay visible={isModalOpen} handleCancel={handleCancel}/>}
      </Suspense>
    </>
  );
};
export default App;

```

- 解决：添加一个判断是否渲染的变量


### 使用场景

- 具有多个路由的 React 应用程序中，可以使用React.lazy和 Suspense 组件为每个路由延迟加载组件。这可以通过仅在用户访问路由时加载路由代码来提高应用程序的性能。

```jsx
import { Route, Switch } from "react-router-dom";
import React, { Suspense, lazy } from "react";
 
const LazyComponent = lazy(() => import("./LazyComponent"));
 
function MyApp() {
  return (
    <Switch>
      <Route
        path="/lazy"
        render={() => (
          <Suspense fallback={<div>Loading...</div>}>
            <LazyComponent />
          </Suspense>
        )}
      />
      {/* Other routes */}
    </Switch>
  );
}
```

-  多列表延迟加载

```jsx
import React, { Suspense, lazy } from "react";
 
const LazyComponent = lazy(() => import("./LazyComponent"));
 
function MyList() {
  const items = [1, 2, 3, 4, 5];
 
  return (
    <ul>
      {items.map((item) => (
        <Suspense key={item} fallback={<div>Loading...</div>}>
          <LazyComponent />
        </Suspense>
      ))}
    </ul>
  );
}
```

### React.lazy 源码

```jsx
export function lazy<T>(
	ctor: () => Thenable<{default: T, ...}>, // 返回 Thenable 可以查看 shared/ReactTypes，其实对应 lazyInitializer 各种状态的返回值
): LazyComponent<T, Payload<T>> { // 返回一个懒加载组件

	const payload: Payload<T> = {
		// We use these fields to store the result.
		_status: Uninitialized, // 当前状态
		_result: ctor, // 函数，如 ()=>import('./Modal.jsx')
	};

	const lazyType: LazyComponent<T, Payload<T>> = {
		$$typeof: REACT_LAZY_TYPE,
		_payload: payload,
		_init: lazyInitializer, // 核心代码
	};

	return lazyType;
}
```

```jsx
// 核心的代码
function lazyInitializer<T>(payload: Payload<T>): T {
	// 假如当前状态是没有初始化
	if (payload._status === Uninitialized) {
		// 获取当期结果，此时是 ctor，即()=>import('./xxx.jsx')
		const ctor = payload._result;
		// 执行函数
		const thenable = ctor();
		// Transition to the next state.
		// This might throw either because it's missing or throws. If so, we treat it
		// as still uninitialized and try again next time. Which is the same as what
		// happens if the ctor or any wrappers processing the ctor throws. This might
		// end up fixing it if the resolution was a concurrency bug.
		thenable.then(
			moduleObject => {
			// 无论当前状态是等待还是未初始化, 执行完毕都修改为 成功状态，结果为 组件返回值
			if (payload._status === Pending || payload._status === Uninitialized) {
				// Transition to the next state.
				const resolved: ResolvedPayload<T> = (payload: any);
				resolved._status = Resolved;
				resolved._result = moduleObject;
			}
		},
		error => {
			// 转换状态，报错
			if (payload._status === Pending || payload._status === Uninitialized) {
				// Transition to the next state.
				const rejected: RejectedPayload = (payload: any);
				rejected._status = Rejected;
				rejected._result = error;
			}
		},);

		// 如果当前状态是未初始化，则修改为等待状态，结果为 promise 对象
		if (payload._status === Uninitialized) {
			// In case, we're still uninitialized, then we're waiting for the thenable
			// to resolve. Set it as pending in the meantime.
			const pending: PendingPayload = (payload: any);
			pending._status = Pending;
			pending._result = thenable;
		}
	}


	if (payload._status === Resolved) {
		const moduleObject = payload._result;

		return moduleObject.default;
	} else {
		throw payload._result;
	}
}
```

- React.lazy本质就是 promise
- import 执行就是返回一个 promise 对象，Suspense 组件通过监听 promise 的状态进行选择渲染。比如执行成功后返回 moduleObject.default 组件的默认导出。