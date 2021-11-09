---
title: "Rewrite Object"
date: 2021-11-09T19:35:56+08:00
draft: true
tags: [rewrite]
categories: [JavaScript]
---

```javascript
Object.assignRw = function (target, ...sources) {
  sources.forEach((source) => {
    const descriptors = Object.keys(source).reduce((descriptor, key) => {
      descriptor[key] = Object.getOwnPropertyDescriptor(source, key)
      return descriptor
    }, {})
    Object.defineProperties(target, descriptors)
  })

  return target
}

Object.entriesRw = function (origin) {
  var _pool = []

  if (Object.prototype.toString.call(origin) === '[object Object]' || Object.prototype.toString.call(origin) === '[object Array]') {
    for (var k in origin) {
      if (origin.hasOwnProperty(k)) {
        var _arr = [k, origin[k]]
        _pool.push(_arr)
      }
    }
  }

  return _pool
}

Object.fromEntriesRw = function (origin) {
  var _obj = {}

  for (var item of origin) {
    _obj[item[0]] = item[1]
  }

  return _obj
}

Object.deepFreeze = function (origin) {
  var _keys = Object.getOwnPropertyNames(origin)

  if (_keys.length > 0) {
    _keys.forEach(function (key) {
      var _value = origin[key]

      if (typeof _value === 'object' && _value !== null) {
        Object.deepFreeze(_value)
      }
    })
  }

  return Object.freeze(origin)
}
```

