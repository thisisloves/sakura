---
title: File对象转换Blob
date: 2020-8-20
tags: JS
author: 映雪
---

以后你遇到的人，都是心里装着别人的人，没有人会用全部来爱你。听闻过往，十忆九悲，听闻爱情，十人九伤！

<!--more-->

- 上传 file

```html
 <Dragger
    name="file"
    fileList={fileList}
    onChange={onFileChange}
    beforeUpload={(file) => {
      let reader = new FileReader();
      reader.readAsText(file); //读取上传文件的内容
        return false; //阻止上传
    }}
  >
    <p className="ant-upload-drag-icon">
      <InboxOutlined />
    </p>
    <p className="ant-upload-text">点击或拖拽上传</p>
</Dragger>
```

- fileList 数据转换 Blob

```js
const formData = new FormData();
fileList.forEach((file) => {
  formData.append('screen_file', file.originFileObj);
  formData.append('screen_id', editData.id);
});
```
