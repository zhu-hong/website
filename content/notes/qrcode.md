---
title: "二维码项目总结"
date: 2022-02-04T09:32:33+08:00
draft: true
---


## 二维码改版

- 测试账号及应用
    
    account: 19142044352
    
    password: Zhgg52000
    
    - 登录，拿到cookie中的Authorization
    - 添加到本地环境中的cookie

Authorization：iCoSobyDk4Q1OdNjYeeL8iIgzBv0GUBjslTpPLHuc7UwK9-it_j5yLIBtaO-zMFS

环境api列表：[https://dl-res.effio.cn/envList.json](https://dl-res.effio.cn/envList.json)

后端根据request中的header中的Authorization判断你是谁，所以每次请求都带上这个Authorization

- 开发者平台使用cookie中的
- 二维码应用使用sessionStorage中的

## API

### 获取二维码列表

/QRCode/QRCode/QueryList?type=0&key=&state=-1&source=-1&creater=&sortType=0&page=1&num=10

state（是否启用禁用）：1禁用0启用

source（文本还是app码）：1文本0app

### 获取二维码详情

/QRCode/QRCode/Info?id=4R5kWZs83N2

### 获取app事件

/QRCode/QRCode/AppEventList

### 获取二维码数据来源

/QRCode/QRCode/AppInstanceList

### 保存单个二维码

```jsx
async saveQrCodeImg () {
  html2canvas(document.getElementById('qrCode'),{
    backgroundColor: 'rgba(0,0,0,0)',
    allowTaint: true,
    scrollX: 0,
    scrollY: 0
  }).then((canvas) => {
	  let dataurl = canvas.toDataURL()
	  let arr = dataurl.split(',');
	  let mime = arr[0].match(/:(.*?);/)[1];
	  let bstr = atob(arr[1]);
	  let n = bstr.length;
	  let u8arr = new Uint8Array(n);
		while (n--) {
		  u8arr[n] = bstr.charCodeAt(n);
		}
	  let file = new File([u8arr], '', {type: mime})
	  saveSync(file,this.previewCode.name + '.png')
})
```

### 保存多个二维码打包思路

把选中的二维码存在dom上，不显示在视口

通过二维码id获取domshi yhtml2canvas转换成base64，循环添加到jszip.folder.file({base64: true})中

### 技术选型

展示二维码 [https://www.npmjs.com/package/qrcode.vue](https://www.npmjs.com/package/qrcode.vue)

下载单个二维码 [https://www.npmjs.com/package/html2canvas](https://www.npmjs.com/package/html2canvas)

下载多个二维码 [https://www.npmjs.com/package/jszip](https://www.npmjs.com/package/jszip)

### 打印

1. frame.contentWindow.document.append(qrcode)
2. frame.contentWindow.print()

### 企业内应用

获取基础配置信息（get） [http://192.168.200.7:30008/goods/InnerApp/GetDetail?id=3vehYXEpnv3](http://192.168.200.7:30008/goods/InnerApp/GetDetail?id=3vehYXEpnv3)

更新密钥（post：id）[http://192.168.200.7:30008/goods/InnerApp/UpdateAppSecret](http://192.168.200.7:30008/goods/InnerApp/UpdateAppSecret)

企业内/goods/innerApp/GetQrDataSource

第三方/goods/qrcode/GetQrDataSource

### 2021-12-20的问题

```jsx
clients: this.clients.map(({ title, id }) => {
  const obj = data.clients.find(({ clientType }) => clientType === id)
  if (obj) {
	  return {
      id,
      title,
      isOpen: true,
      testAddress: obj.entranceAddress,
      description: obj.description,
      isOpenAll: obj.isOpenAll
	   }
   } else {
     return {
       id,
       title,
       isOpen: false,
       testAddress: '',
	     description: '',
	     isOpenAll: false
	   }
	 }
})
```

最后clients有时为空有时又有值，我以为是处理的是异步数据的问题，this.clients的值在另一个异步请求中，所以操作的时候，this.clients可能还没请求成功

### element-ui表单验证bug

rules的字段prop要和v-model的字段一样

### 二维码预览数据url

[http://192.168.50.33:5000/goods/InnerApp/GetMockQrDataSource](http://192.168.50.33:5000/goods/InnerApp/GetMockQrDataSource)

### 正则匹配URL

```jsx
const urlReg = /(https?|ftp|file):\/\/[-A-Za-z0-9+&@##/%?=~_|!:,.;]+[-A-Za-z0-9+&@##/%=~_|]/;
```

### 柯里化

```jsx
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

export const getQrSources = () => {
  return new Promise((resolve) => {
    Qr.get('/GetQrAppGroupList')
      .then(({ data: { data } }) => {
        resolve(data)
      })
  })
}
```

### element-ui

table-column里放dropdown需要slot-scope不然无效果

### 多级动态表单验证错误定位

### 是否为管理员生成

isSystem：true 为二维码生成器生成

### NULL

把null当成对象访问null的属性会报错

### Computed

异步不行

### 2022-1-7(第一周总结)

目前负责二维码相关改版工作,开发者后台改版工作已经完成,正在进行二维码web端的改版工作

本周为止完成了二维码的列表展示,条件筛选,单个创建,多个创建,部分编辑..等功能

下周准备完成预览二维码预览,标签排版,拆分组件

### 分页数据问题

从后往前删,删完最后一页最后一个数据会显示空数据

```jsx
async getQrList() {
	this.loading = true
	const { data, total } = await Qr.getList(this.args)
	this.loading = false
	
	if(data.length === 0 && total !== 0 && this.condition.page !== 1) {
	  this.condition.page--
	  this.getQrList()
	  return
	}
	this.qrList = { data, total } 
}
```

### nextTick

css3 transtion会造成nextTick无法正常使用

### getBoundingClientRect

元素display为none时getBoundingClientRect获取到的尺寸为0

### 二维码下载logo损坏bug

二维码列表我做了缓存处理，如果缓存有的话，直接转换缓存中的DOM为Canvas下载（如果图片src必须为base6），缓存来源三个操作（预览/下载/打印），下载和打印会将logo转换为base64，而预览不会，所以通过预览二维码缓存，logo还是网络地址，所以htc logo失败

### JSZip

async await返回的new JSZip没用，需要reslove，如下

```jsx
if(Object.keys(qrZip.files).length === this.selectedQrs.length) {
  resolve(qrZip)
}
```

### html2canvas

这个库的运行会向页面添加iframe期间页面无法操作，并且要等所有html2canvas完成才会开始将iframe移除，同时运行一两个html2canvas还好，如果同时运行两位数拿它同样会往页面上添加两位数的iframe，这就不单单是页面无法操作了，可能直接崩溃，所以我使用了p-limit来进行并发控制，直接限制一次只能运行一个html2canvas，这一个完成了才能进行下一个

### 性能优化

打包优化

1. 按需加载
2. 小文件直接转base64
3. tree-shaking
4. externals外部拓展
5. splitChunks分包

开发优化

1. 减少http请求，图片base64，雪碧图，懒加载，发抖/节流
2. 长列表优化，pc端：数据分页，移动端：虚拟滚动
3. 减少回流，移动元素时使用不影响布局的方法（transform...）
4. Vue优化，添加列表key
5. 并发控制，p-limit + Promise.all
6. 缓存（强/协商缓存）
7. 前端对象缓存池

### 415

请求格式错误/请求参数放错了地方