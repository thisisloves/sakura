---
title: File对象
date: 2019-4-12
tags: JS
author: 映雪
---

你凭栏而待，眉目自成诗三百，鬓如春风裁。

<!-- more-->

### Fild 对象

- File 对象代表一个文件，用来读写文件信息。它继承了 Blob 对象，或者说是一种特殊的 Blob 对象，所有可以使用 Blob 对象的场合都可以使用它。
- 最常见的使用场合是表单的文件上传控件（`<input type="file">`），用户选中文件以后，浏览器就会生成一个数组，里面是每一个用户选中的文件，它们都是 File 实例对象。

```html
<body>
  <input type="file" accept="image/*" multiple onchange="fileinfo(this.files)" />
</body>
<script>
  function fileinfo(files) {
    for (var i = 0; i < files.length; i++) {
      var f = files[i];
      console.log(f); // File 对象
    }
  }
</script>
```

### File 语法

- 浏览器原生提供一个 File()构造函数，用来生成 File 实例对象。

```js
new File(array, name [, options])
```

- File()构造函数接受三个参数。

- array：一个数组，成员可以是二进制对象或字符串，表示文件的内容。
- name：字符串，表示文件名或文件路径。
- options：配置对象，设置实例的属性。该参数可选。
- 第三个参数配置对象，可以设置两个属性。

- type：字符串，表示实例对象的 MIME 类型，默认值为空字符串。
- lastModified：时间戳，表示上次修改的时间，默认为 Date.now()。

```js
var file = new File(['foo'], 'foo.txt', {
  type: 'text/plain',
});
```

### File 实例属性和实例方法

1. File.lastModified

- 最后修改时间

2. File.name

- 文件名或文件路径

3. File.size

- 文件大小（单位字节）

4. File.type

- 文件的 MIME 类型

```js
var myFile = new File([], 'file.bin', {
  lastModified: new Date(2018, 1, 1),
});
myFile.lastModified; // 1517414400000
myFile.name; // "file.bin"
myFile.size; // 0
myFile.type; // ""
```

### FileList 对象

- FileList 对象是一个类似数组的对象，代表一组选中的文件，每个成员都是一个 File 实例。它主要出现在两个场合。

- 文件控件节点（`<input type="file">`）的 files 属性，返回一个 FileList 实例。
- 拖拉一组文件时，目标区的 DataTransfer.files 属性，返回一个 FileList 实例。

### FileRead 对象

- leReader 对象用于读取 File 对象或 Blob 对象所包含的文件内容。

- 浏览器原生提供一个 FileReader 构造函数，用来生成 FileReader 实例。

```js
var reader = new FileReader();
```

### FileReader 实例属性

|          属性          |                                                   描述                                                   |
| :--------------------: | :------------------------------------------------------------------------------------------------------: |
|    FileReader.error    |                                         读取文件时产生的错误对象                                         |
| FileReader.readyState  | 整数，表示读取文件时的当前状态。有三种状态，0 表示尚未加载任何数据，1 表示数据正在加载，2 表示加载完成。 |
|   FileReader.result    |                  读取完成后的文件内容，有可能是字符串，也可能是一个 ArrayBuffer 实例。                   |
|   FileReader.onabort   |                                abort 事件（用户终止读取操作）的监听函数。                                |
|   FileReader.onerror   |                                    error 事件（读取错误）的监听函数。                                    |
|   FileReader.onload    |         load 事件（读取操作完成）的监听函数，通常在这个函数里面使用 result 属性，拿到文件内容。          |
| FileReader.onloadstart |                                loadstart 事件（读取操作开始）的监听函数。                                |
|  FileReader.onloadend  |                                 loadend 事件（读取操作结束）的监听函数。                                 |
| FileReader.onprogress  |                               progress 事件（读取操作进行中）的监听函数。                                |

### FileReader 实例方法

|              方法               |                                          描述                                          |
| :-----------------------------: | :------------------------------------------------------------------------------------: |
|       FileReader.abort()        |                        终止读取操作，readyState 属性将变成 2。                         |
| FileReader.readAsArrayBuffer()  |   以 ArrayBuffer 的格式读取文件，读取完成后 result 属性将返回一个 ArrayBuffer 实例。   |
| FileReader.readAsBinaryString() |                    取完成后，result 属性将返回原始的二进制字符串。                     |
|   FileReader.readAsDataURL()    | 读取完成后，result 属性将返回一个 Data URL 格式（Base64 编码）的字符串，代表文件内容。 |
|     FileReader.readAsText()     |                   取完成后，result 属性将返回文件内容的文本字符串。                    |
