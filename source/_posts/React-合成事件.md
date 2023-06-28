---
title: React合成事件
date: 2019-6-5
tags: React
author: 映雪
---

世间一切幸福，皆月影中一现的昙花；唯有孤独与痛，常伴在黄泉深处。

<!--more-->

### 合成事件

- React 自己实现了一套高效的事件注册，存储，分发和重用逻辑，在 DOM 事件体系基础上做了很大改进，减少了内存消耗，简化了事件逻辑，并最大化的解决了 IE 等浏览器的不兼容问题。

### 合成事件特点

1. React 组件上声明的事件最终绑定到了 document 这个 DOM 节点上，而不是 React 组件对应的 DOM 节点。故只有 document 这个节点上面才绑定了 DOM 原生事件，其他节点没有绑定事件。这样简化了 DOM 原生事件，减少了内存开销。
2. React 以队列的方式，从触发事件的组件向父组件回溯，调用它们在 JSX 中声明的 callback。也就是 React 自身实现了一套事件冒泡机制。我们没办法用 event。stopPropagation()来停止事件传播，应该使用 event.preventDefault()。
3. React 有一套自己的合成事件 SyntheticEvent，不同类型的事件会构造不同的 SyntheticEvent。
4. React 使用对象池来管理合成事件对象的创建和销毁，这样减少了垃圾的生成和新对象内存的分配，大大提高了性能。

### 合成事件的原理

- 通过冒泡事件冒泡到顶层元素，再统一分发处理。

![截屏2022-05-06 上午10.07.12.png](/images/2022/05/06/hu4xEkMODP5bGdF.png)

### react 事件系统

![截屏2022-05-06 下午2.30.30.png](/images/2022/05/06/7mcKb5TeU196dPr.png)

- 浏览器事件（如用户点击了某个 button）触发后，DOM 将 event 传给 ReactEventListener，它将事件分发到当前组件及以上的父组件。然后由 ReactEventEmitter 对每个组件进行事件的执行，先构造 React 合成事件，然后以 queue 的方式调用 JSX 中声明的 callback 进行事件回调。

- 涉及主要的类

1. ReactEventListener：负责事件注册和事件分发。React 将 DOM 事件全都注册到 document 这个节点上，这个我们在事件注册小节详细讲。事件分发主要调用
   dispatchEvent 进行，从事件触发组件开始，向父元素遍历。我们在事件执行小节详细讲。

2. ReactEventEmitter：负责每个组件上事件的执行。

3. EventPluginHub：负责事件的存储，合成事件以对象池的方式实现创建和销毁，大大提高了性能。

4. SimpleEventPlugin 等 plugin：根据不同的事件类型，构造不同的合成事件。如 focus 对应的 React 合成事件为 SyntheticFocusEvent

### 事件注册

- react 声明事件

```js
render() {
  return (
    <div onClick = {
            (event) => {console.log(JSON.stringify(event))}
        }
    />
  );
}
```

- enqueuePutListener，它负责注册 JSX 中声明的事件

```js
// inst: React Component对象
// registrationName: React合成事件名，如onClick
// listener: React事件回调方法，如onClick=callback中的callback
// transaction: mountComponent或updateComponent所处的事务流中，React都是基于事务流的
function enqueuePutListener(inst, registrationName, listener, transaction) {
  if (transaction instanceof ReactServerRenderingTransaction) {
    return;
  }
  var containerInfo = inst._hostContainerInfo;
  var isDocumentFragment = containerInfo._node && containerInfo._node.nodeType === DOC_FRAGMENT_TYPE;
  // 找到document
  var doc = isDocumentFragment ? containerInfo._node : containerInfo._ownerDocument;
  // 注册事件，将事件注册到document上
  listenTo(registrationName, doc);
  // 存储事件,放入事务队列中
  transaction.getReactMountReady().enqueue(putListener, {
    inst: inst,
    registrationName: registrationName,
    listener: listener
  });

```

- enqueuePutListener 主要做两件事，一方面将事件注册到 document 这个原生 DOM 上（这就是为什么只有 document 这个节点有 DOM 事件的原因），另一方面采用事务队列的方式调用 putListener 将注册的事件存储起来，以供事件触发时回调。
- 注册事件的入口是 listenTo 方法, 它解决了不同浏览器间捕获和冒泡不兼容的问题。在 listen 方法中只有 document 节点才会调用这个 addEventListener 这个原生事件注册方法，故仅仅只有 document 节点上才有 DOM 事件。这大大简化了 DOM 事件逻辑，也节约了内存。

### 事件存储

- 事件存储由 EventPluginHub 来负责，它的入口在 enqueuePutListener 中的 putListener 方法

```js

  /**
   * EventPluginHub用来存储React事件, 将listener存储到`listenerBank[registrationName][key]`
   *
   * @param {object} inst: 事件源
   * @param {string} listener的名字,比如onClick
   * @param {function} listener的callback
   */
  //
  putListener: function (inst, registrationName, listener) {

    // 用来标识注册了事件,比如onClick的React对象。key的格式为'.nodeId', 只用知道它可以标示哪个React对象就可以了
    var key = getDictionaryKey(inst);
    var bankForRegistrationName = listenerBank[registrationName] || (listenerBank[registrationName] = {});
    // 将listener事件回调方法存入listenerBank[registrationName][key]中,比如listenerBank['onclick'][nodeId]
    // 所有React组件对象定义的所有React事件都会存储在listenerBank中
    bankForRegistrationName[key] = listener;

    //onSelect和onClick注册了两个事件回调插件, 用于walkAround某些浏览器兼容bug,不用care
    var PluginModule = EventPluginRegistry.registrationNameModules[registrationName];
    if (PluginModule && PluginModule.didPutListener) {
      PluginModule.didPutListener(inst, registrationName, listener);
    }
  },

var getDictionaryKey = function (inst) {
  return '.' + inst._rootNodeID;

```

- 事件存储在了 listenerBank 对象中，它按照事件名和 React 组件对象进行了二维划分。

### 事件执行分发

- 当事件触发的，document 上的 callback 会被回调。从前面事件注册部分发现，此时回调函数为 ReactEventListener.dispatchEvent，它是事件分发的入口方法。

```js
var ReactEventListener = {
  // topLevelType：事件名
  // nativeEvent: 用户触发click等事件时，浏览器传递的原生事件
  dispatchEvent: function (topLevelType, nativeEvent) {
    ...
   // 放入批处理队列中,React事件流也是一个消息队列的方式
    ReactUpdates.batchedUpdates(handleTopLevelImpl, bookKeeping);
    ...
  }
}
//handleTopLevelImpl才是事件分发的真正执行者，它是事件分发的核心，体现了React事件分发的特点
function handleTopLevelImpl(bookKeeping) {
  //根据原生的事件对象，找到事件触发的dom元素以及该dom对应的component对象
  var nativeEventTarget = getEventTarget(bookKeeping.nativeEvent);
  var targetInst = ReactDOMComponentTree.getClosestInstanceFromNode(nativeEventTarget);
 // 执行事件回调前,先由当前组件向上遍历它的所有父组件。得到ancestors这个数组。
  // 因为事件回调中可能会改变Virtual DOM结构,所以要先遍历好组件层级
  var ancestor = targetInst;
  do {
    bookKeeping.ancestors.push(ancestor);
    ancestor = ancestor && findParent(ancestor);
  } while (ancestor);

  // 从当前组件向父组件遍历,依次执行注册的回调方法. 我们遍历构造ancestors数组时,是从当前组件向父组件回溯的,故此处事件回调也是这个顺序
  // 这个顺序就是冒泡的顺序,并且我们发现不能通过stopPropagation来阻止'冒泡'。
  for (var i = 0; i < bookKeeping.ancestors.length; i++) {
    targetInst = bookKeeping.ancestors[i];
     // _handleTopLevel是初始化时用ReactEventEmitterMixin注入进来的
    ReactEventListener._handleTopLevel(bookKeeping.topLevelType, targetInst, bookKeeping.nativeEvent, getEventTarget(bookKeeping.nativeEvent));
  }
}

// ReactEventEmitterMixin.js
// ReactEventEmitterMixin一方面生成合成的事件对象，另一方面批量执行定义的回调函数
var ReactEventEmitterMixin = {
  // handleTopLevel方法是事件回调函数调用的核心。DOM事件绑定在了document原生对象上,每次事件触发,都会调用到handleTopLevel
  handleTopLevel: function (...) {
    // 利用浏览器回传的原生事件构造出React合成事件。不同的eventType的合成事件可能不同
    var events = EventPluginHub.extractEvents(...);
    //采用队列的方式处理回调函数
    runEventQueueInBatch(events);
  }
}
//进行批量更新
function runEventQueueInBatch(events) {
  EventPluginHub.enqueueEvents(events);
  EventPluginHub.processEventQueue(false);
}
```

- React 自身实现了一套冒泡机制。从触发事件的对象开始，向父元素回溯，依次调用它们注册的事件 callback。

### 生成合成事件

```js
// EventPluginHub.js
var EventPluginHub = {
  extractEvents: function (...) {
    var events;
    // EventPluginHub可以存储React合成事件的callback,也存储了一些plugin,这些plugin在EventPluginHub初始化时注册的
    var plugins = EventPluginRegistry.plugins;
    for (var i = 0; i < plugins.length; i++) {
      var possiblePlugin = plugins[i];
      if (possiblePlugin) {
        // 根据eventType构造不同的合成事件SyntheticEvent
        var extractedEvents = possiblePlugin.extractEvents(topLevelType, targetInst, nativeEvent, nativeEventTarget);
        if (extractedEvents) {
          // 将构造好的合成事件extractedEvents添加到events数组中,这样就保存了所有plugin构造的合成事件
          events = accumulateInto(events, extractedEvents);
        }
      }
    }
    return events;
  }
}
```

- 系统启动过程中注入(injection)过来 plugins，代码如下：

```js
// react-dom模块的入口文件ReactDOM.js:
var ReactDefaultInjection = require('./ReactDefaultInjection');
ReactDefaultInjection.inject();
...
// ReactDefaultInjection.js
module.exports = {
  inject: inject
};
function inject() {
  ...
  ReactInjection.EventPluginHub.injectEventPluginsByName({
    SimpleEventPlugin: SimpleEventPlugin,
    EnterLeaveEventPlugin: EnterLeaveEventPlugin,
    ChangeEventPlugin: ChangeEventPlugin,
    SelectEventPlugin: SelectEventPlugin,
    BeforeInputEventPlugin: BeforeInputEventPlugin
  });
  ...
}
```

- 默认情况下，react 注入了五种事件 plugin，针对不同的事件，得到不同的合成事件，下面看一下最常见的 SimpleEventPlugin 如何生成它对应的 React 合成事件，代码如下：

```js
 // 根据不同事件类型,比如click,focus构造不同的合成事件SyntheticEvent, 如SyntheticKeyboardEvent SyntheticFocusEvent
extractEvents: function (topLevelType, targetInst, nativeEvent, nativeEventTarget) {
    var dispatchConfig = topLevelEventsToDispatchConfig[topLevelType];
    if (!dispatchConfig) {
      return null;
    }
    var EventConstructor;

   // 根据事件类型，采用不同的SyntheticEvent来构造不同的合成事件
    switch (topLevelType) {
      ... // 仅以blur和focus为例
      case 'topBlur':
      case 'topFocus':
        EventConstructor = SyntheticFocusEvent;
        break;
      ...
    }

    // 从event对象池中取出合成事件对象
    var event = EventConstructor.getPooled(dispatchConfig, targetInst, nativeEvent, nativeEventTarget);
   //用于从EventPluginHub中获取回调函数
    EventPropagators.accumulateTwoPhaseDispatches(event);
    return event;
},
```

- 调用 EventPropagators.accumulateTwoPhaseDispatches(event)从 EventPluginHub 中获取回调函数，如何获取具体的回调函数，如下：

```js
// EventPropagators.js
function accumulateTwoPhaseDispatches(events) {
  forEachAccumulated(events, accumulateTwoPhaseDispatchesSingle);
}
function accumulateTwoPhaseDispatchesSingle(event) {
  if (event && event.dispatchConfig.phasedRegistrationNames) {
    EventPluginUtils.traverseTwoPhase(event._targetInst, accumulateDirectionalDispatches, event);
  }
}
function accumulateDirectionalDispatches(inst, phase, event) {
  var listener = listenerAtPhase(inst, event, phase);
  if (listener) {
    event._dispatchListeners = accumulateInto(event._dispatchListeners, listener);
    event._dispatchInstances = accumulateInto(event._dispatchInstances, inst);
  }
}
var getListener = EventPluginHub.getListener;
function listenerAtPhase(inst, event, propagationPhase) {
  var registrationName = event.dispatchConfig.phasedRegistrationNames[propagationPhase];
  return getListener(inst, registrationName);
}
// EventPluginHub.js
getListener: function (inst, registrationName) {
  var bankForRegistrationName = listenerBank[registrationName];
  var key = getDictionaryKey(inst);
  return bankForRegistrationName && bankForRegistrationName[key];
},
```

### 批量处理回调函数

- 回调函数的执行为两步，源码如下：

```js
function runEventQueueInBatch(events) {
  // 第一步：将events事件放入队列中
  EventPluginHub.enqueueEvents(events);
  // 第二部：处理队列中的事件,包括之前未处理完的。先入先处理原则
  EventPluginHub.processEventQueue(false);
}
```

- 第一步源码如下：

```js
var eventQueue = null;
var EventPluginHub = {
  enqueueEvents: function (events) {
    if (events) {
      eventQueue = accumulateInto(eventQueue, events);
    }
  },
  processEventQueue: function (simulated) {
    var processingEventQueue = eventQueue;
    ...
    forEachAccumulated(processingEventQueue, executeDispatchesAndReleaseSimulated);
    ...
  },
}

function accumulateInto(current, next) {

  if (current == null) {
    return next;
  }

  // 将next添加到current中,返回一个包含他们两个的新数组
  // 如果next是数组,current不是数组,采用push方法,否则采用concat方法
  // 如果next不是数组,则返回一个current和next构成的新数组
  if (Array.isArray(current)) {
    if (Array.isArray(next)) {
      current.push.apply(current, next);
      return current;
    }
    current.push(next);
    return current;
  }

  if (Array.isArray(next)) {
    return [current].concat(next);
  }

  return [current, next];
}
```

- 第二步事件执行的入口方法为 executeDispatchesAndReleaseTopLevel，代码如下：

```js
var executeDispatchesAndReleaseTopLevel = function (e) {
  return executeDispatchesAndRelease(e, false);
};
var executeDispatchesAndRelease = function (event, simulated) {
  if (event) {
    //进行事件分发
    EventPluginUtils.executeDispatchesInOrder(event, simulated);

    if (!event.isPersistent()) {
      // 处理完,则release掉event对象,采用对象池方式,减少GC
      // React帮我们处理了合成事件的回收机制，不需要我们关心。但要注意，如果使用了DOM原生事件，则要自己回收
      event.constructor.release(event);
    }
  }
};
// EventPluginUtils.js
// 事件处理的核心
function executeDispatchesInOrder(event, simulated) {
  var dispatchListeners = event._dispatchListeners;
  var dispatchInstances = event._dispatchInstances;
  if (Array.isArray(dispatchListeners)) {
    // 如果有多个listener,则遍历执行数组中event
    for (var i = 0; i < dispatchListeners.length; i++) {
      // 如果isPropagationStopped设成true了,则停止事件传播,退出循环。
      if (event.isPropagationStopped()) {
        break;
      }
      // 执行event的分发,从当前触发事件元素向父元素遍历
      // event为浏览器上传的原生事件
      // dispatchListeners[i]为JSX中声明的事件callback
      // dispatchInstances[i]为对应的React Component
      executeDispatch(event, simulated, dispatchListeners[i], dispatchInstances[i]);
    }
  } else if (dispatchListeners) {
    // 如果只有一个listener,则直接执行事件分发
    executeDispatch(event, simulated, dispatchListeners, dispatchInstances);
  }
  // 处理完event,重置变量。因为使用的对象池,故必须重置,这样才能被别人复用
  event._dispatchListeners = null;
  event._dispatchInstances = null;
}
```
