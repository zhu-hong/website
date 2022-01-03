---
title: "柯里化"
date: 2022-01-03T18:04:38+08:00
draft: true
tags: [currying]
---

## 历史

柯里化是计算机界的一个概念或者说编程技巧而不是具体的技术,一年前学习JS[(currying)](https://www.bilibili.com/video/BV1P741147CT?p=37)的时候没当回事,就感觉这东西有啥用啊,好家伙是我境界还没到,最近终于是体会到了

## 缘起

先看看以下代码,这是一段封装`axios`的代码

```javascript
import axios from 'axios'
import { Message, MessageBox } from 'element-ui'
import { toHomePage } from '.'

const Basic = axios.create()

Basic.interceptors.request.use((config) => {

  config.headers = {
    'Authorization': '53N3Ce8LQ3WYuyL2dH3wGozxkSG98gjgLvmAnGGpoIHPH66gCzlQAhO6a3V9-r2i',
  }

  return config
})

Basic.interceptors.response.use((response) => {
  let { status } = response
  if((status >= 200 && status <= 204) || status === 304) {
    return response
  }
  return Promise.reject(response)
}, (error) => {
  let { response: { status } } = error
  switch(status) {
    case 401:
      MessageBox.confirm('登录失效，请重新登录', '提示', {
        type: 'error',
        confirmButtonText: '确定',
        showCancelButton: false,
        callback: toHomePage,
      })
      break
    case 403:
      Message({
        type: 'error',
        message: '无权限',
      })
      throw Error('UnauthorizedException')
    default:
      Message({
        type: 'error',
        message: '无网络',
      })
      break
  }
})

// 获取用户信息
export const getMe = () => {
  return new Promise((resolve) => {
    Basic.get(process.env.VUE_APP_USER_HOST + '/Account/User/GetMe')
      .then(({ data: { data } }) => {
        resolve({
          name: data.name,
          avatar: data.profilePicture,
        })
      })
  })
}

const Qr = axios.create({
  baseURL: process.env.VUE_APP_MAIN_HOST + '/qr/QR',
})

Qr.interceptors.request.use((config) => {

  config.headers = {
    'Authorization': '53N3Ce8LQ3WYuyL2dH3wGozxkSG98gjgLvmAnGGpoIHPH66gCzlQAhO6a3V9-r2i',
  }

  return config
})

Qr.interceptors.response.use((response) => {
  let { status } = response
  if((status >= 200 && status <= 204) || status === 304) {
    return response
  }
  return Promise.reject(response)
}, (error) => {
  let { response: { status } } = error
  switch(status) {
    case 401:
      MessageBox.confirm('登录失效，请重新登录', '提示', {
        type: 'error',
        confirmButtonText: '确定',
        showCancelButton: false,
        callback: toHomePage,
      })
      break
    case 403:
      Message({
        type: 'error',
        message: '无权限',
      })
      throw Error('UnauthorizedException')
    default:
      Message({
        type: 'error',
        message: '无网络',
      })
      break
  }
})

// 获取二维码列表
export const getQrList = (params) => {
  return new Promise((resolve) => {
    Qr.get('/GetList', { params })
      .then(({ data }) => {
        resolve(data)
      })
  })
}
```

我先来说一下这段代码的情况

1. 我现在有两套接口要请求,一套是获取用户信息,一套是获取二维码数据
2. 两套接口使用的请求/响应拦截器规则是同一套,但是BaseURL不一样
3. 我又贪图配个baseURL后面拼短的`url`方便,我就创建了两个axios实例不同的baseURL

当时写的时候就觉得问题很大,非常得不优雅很蛋疼,但是当时也没有想到更好的解决方案,就先这样吧

## 思考

上面这段代码的问题就是重复性太高了,复用同一套规则,但肯定不是这样复用,这么写一定有问题,当时一时半会也想不出来更好的方案,索性就先这样写,严格来说这不是问题,只是没优化好,先解决需求再优化

任何下班在路上我就一直在思考这个问题

+ 一个axios实例
+ 一个请求/响应规则
+ 多个baseURL

或许可以把规则抽离成对象,这样能少些代码,但是也不行,本质和原本的代码是一样的

然后走到快到住的地方的那座桥,突然灵光乍现想到了一个解决方案,这不就相当于是柯里化吗,然后就有了下面这段代码

```javascript
import axios from 'axios'

const axiosCurrying = (baseURL) => {
  return (method) => {
    return (url, args) => {
      return new Promise(async (resolve) => {
        let config = {
          baseURL,
          method,
          url,
        }
        if (method.toUpperCase() === 'POST') {
          config['data'] = args
        } else {
          config['params'] = args
        }
        let { data } = await axios(config)
        resolve(data)
      })
    }
  }
}

const jsphd = axiosCurrying('https://jsonplaceholder.typicode.com')

const jsphdGet = jsphd('get')

jsphdGet('posts', {
  _limit: 5
}).then((data) => {
  console.log(data)
})
```

上面这段代码我觉得是完美解决了请求/响应拦截的复用和多个baseURL的问题,或许能再优化一下,结合实际情况,公司的后端接口请求方式都是`get`和`post`方法,可以返回一个对象,对象有get/post两个方法,这样就不用第二次传参数选择需要使用何种方法

最终版的代码如下

```javascript
import axios from 'axios'
import { Message, MessageBox } from 'element-ui'
import { toHomePage } from '.'

export const token = 'goFHpGnOa6eqsqH4lraP_1hek5Q5Q_S3x8g4WN0OCQ0WRNFZ-ywrlWI76WX8LkUx'

const Basic = axios.create()

Basic.interceptors.request.use((config) => {

  config.headers = {
    'Authorization': token,
  }

  return config
})

Basic.interceptors.response.use((response) => {
  let { status } = response
  if((status >= 200 && status <= 204) || status === 304) {
    return response
  }
  return Promise.reject(response)
}, (error) => {
  let { response: { status } } = error
  switch(status) {
    case 401:
      MessageBox.confirm('登录失效，请重新登录', '提示', {
        type: 'error',
        confirmButtonText: '确定',
        showCancelButton: false,
        callback: toHomePage,
      })
      break
    case 403:
      Message({
        type: 'error',
        message: '无权限',
      })
      throw Error('UnauthorizedException')
    default:
      Message({
        type: 'error',
        message: '无网络',
      })
      break
  }
})

export const curryingHTTP = (baseURL) => {
  const config = {
    baseURL,
  }
  return {
    get(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await Basic(Object.assign(config, {
          method: 'GET',
          url,
          params: args,
        }))
        resolve(data)
      })
    },
    post(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await Basic(Object.assign(config, {
          method: 'POST',
          url,
          data: args,
        }))
        resolve(data)
      })
    },
  }
}
```

其中妙处还请自我体会