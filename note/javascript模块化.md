##  umd 规范
```
;(function (root, factory) {
  if (typeof module === 'object' && typeof module.exports === 'object') {
    /* CommonJS规范 */
    module.exports = factory()
  } else if (typeof define === 'function' && define.amd) {
    /* AMD规范 */
    define(factory)
  } else if (typeof define === 'function' && define.cmd) {
    /* CMD规范 */
    define(function (require, exports, module) {
      module.exports = factory()
    })
  } else {
    /* 挂在到全局 global 对象 */
    root.umdModule = factory()
  }
})(this, function () {
  return {
    name: 'weidian'
  }
})
```
