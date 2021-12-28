---
title: "JS经验"
date: 2021-12-27T19:44:43+08:00
draft: true
categories: [JavaScript]
code:
  maxShownLines: 50
---

## axios柯里化

```typescript
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

```javascript
const curryingHTTP = (baseURL) => {
  const config = {
    baseURL,
  }
  return {
    get(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await axios(Object.assign(config, {
          method: 'GET',
          url,
          params: args,
        }))
        resolve(data)
      })
    },
    post(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await axios(Object.assign(config, {
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