---
title: "微信小程序"
date: 2021-12-05T19:26:25+08:00
draft: true
---

## 简述

+ 运行在微信里的应用,即点即用,感觉不到安装的过程,所以对应用大小有严格的限制(`2M`)

+ 逻辑层与渲染层分离
+ 一个`App`一个或多个`Page`

```javascript
// App app.js
APP({})

// Page app.json里注册
{
  "pages": [
    "pages/index/index"
  ]
}
```

+ 一个`Page`由`index.wxml`,`index.wxss`,`index.js`组成,主要还是得在`app.json`中注册

## 动态数据

对应`Page`中的`data`,`响应式`

### 渲染

```
{{var}}
```

### 修改

```js
this.setData({
  key: value,
})
```

## 列表渲染

### 语法

默认为`index` `item`,需要使用别的名字参考如下

```html
<view wx:for={{list}} wx:for-index="idx" wx:for-item="itemName">
  {{idx}}:{{itemName}}
</view>
```

### key

如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如`input`中的输入内容，`switch`的选中状态），需要使用 `wx:key` 来指定列表中项目的唯一的标识符。

保留关键字 `*this` 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字。

## 条件渲染

### 语法

```html
<view wx:if={{true}}></view>
```

### if or hidden

类似于`VUE`中的`v-if` `v-show`

## 模板

```html
<template name="odd">
  <view> odd </view>
</template>
<template name="even">
  <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
  <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
</block>
```

## 网络功能

小程序只能跟指定的域名进行通信`在小程序管理页面配置`

小程序发起网络请求只能使用`wx.request`
