## 实现apply、call和bind函数

``` javascript
Function.prototype.myCall = function (obj) {
  let context = obj || window
  obj.fn = this
  let args = [...arguments].splice(1)
  let result = obj.fn(...args)
  delete obj.fn
  return result
}

Function.prototype.myApply = function (obj) {
  let context = obj || window
  obj.fn = this
  let args = arguments[1]
  let result
  if (args) {
    result = obj.fn(...args)
  } else {
    result = obj.fn()
  }

  delete obj.fn

  return result
}

Function.prototype.myBind = function (obj) {
  let context = obj || window
  let _this = this
  let _args = [...arguments].splice(1)

  return function () {
    let args = arguments
    // 产生副作用
    // return obj.fn(..._args, ...args)
    return _this.apply(context, [..._args, ...args])
  }
}

function myFun (argumentA, argumentB) {
  console.log(this.value)
  console.log(argumentA)
  console.log(argumentB)
  return this.value
}

let obj = {
  value: 'ziyi2'
}
console.log(myFun.myCall(obj, 11, 22))
console.log(myFun.myApply(obj, [11, 22]))
console.log(myFun.myBind(obj, 33)(11, 22))

```


## 防抖

### 简单防抖

``` javascript
function debounce (fn, wait = 1000) {
  let timeOutId

  return function () {
    let context = this

    if (timeOutId) {
      clearTimeout(timeOutId)
    }

    timeOutId = setTimeout(() => {
      fn.apply(context, arguments)
    }, wait)
  }
}
```

### 带立即执行的参数的防抖

``` javascript
function debounceImmediate (fn, wait = 1000, immediate) {
  let timeOutId, context, args

  const later = (immediate) => setTimeout(() => {
    if (!immediate) {
      fn.apply(context, args)
      timeOutId = context = args = null
    }
  }, wait)

  return function () {
    if (!timeOutId) {
      timeOutId = later(true)

      if (immediate) {
        fn.apply(this, arguments)
      }

      context = this
      args = arguments
    } else {
      clearTimeout(timeOutId)
      timeOutId = later(false)
    }
  }
}
```

## 节流

``` javascript
function throttle (fn, wait) {
  let timeoutId = null
  return function () {
    let context = this
    if (!timeoutId) {
      timeoutId = setTimeout(() => {
        fn.apply(context, arguments)
        timeoutId = null
      }, wait)
    }
  }
}
```



## 转义

``` javascript
function htmlEncode (html) {
  return html.replace(/[<>"'&]/g, function (match) {
    switch (match) {
      case '<':
        return '&lt;'
      case '>':
        return '&gt;'
      case '&':
        return '&amp;'
      case "'":
        return '&#39;'
      case '"':
        return '&quot;'
    }
  })
}
```




