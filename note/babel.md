babel 就是一个编译器（输入源码 -> 输出编译后的代码），就像其他编译器一样，编译过程分为三个阶段： 解析，转换和打印输出。

babel 虽然开箱即用，但他什么都不做，类似于`const babel = code => code`，即将代码解析之后，再输出同样的代码。如果想让 babel 做一些事情，就需要为其指定插件。

除了一个一个的添加插件，也可以使用 preset 来启用一组插件。

##  插件
插件分为两类： 
* 转换插件
* 语法插件。

转换插件： 转换插件用于转换源代码，转换插件将自动启用相应的语法插件，因此不必同时指定这两种插件。

语法插件： 语法插件只允许 babel 解析（Parse）特定类型的语法（而不是转换）

###  插件/preset 路径
如果插件在是通过 npm 安装的。你可以输入插件的名称，babel 会自动检查它是否已经被安装到 node_modules 目录下：
```
{
  "plugins": ["babel-plugin-myPlugin"]
}
```
也可以指定插件的相对/绝对路径
```
{
  "plugins": ["./node_modules/asdf/plugin"]
}
```

### 插件名的简写

如果插件名称的前缀是`babel-plugin-`，则可以省略`babel-plugin-`,例如：
```
{
  "plugins": [
    "myPlugin",
    "babel-plugin-myPlugin" // 两个插件实际是同一个
  ]
}
```
这也适用于带有冠名的插件
```
{
  "plugins": [
    "@org/babel-plugin-name",
    "@org/name" // 两个插件实际是同一个
  ]
}
```
### 插件的顺序
插件的排列顺序很重要，这意味着如果两个转换插件豆浆处理程序的某个代码片段，则将根据转货插件或者 preset 的排列顺序依次执行：
* 插件在 presets 前运行。
* 插件顺序从前往后。
* presets顺序是颠倒的（从后往前）。

例如：
```
{
  "plugins": ["transform-decorators-legacy", "transform-class-properties"]
}
```
先执行`transform-decorators-legacy`，再执行`transform-class-properties`。
```
{
  "presets": ["es2015", "react", "stage-2"]
}
```
presets 将按照如下顺序执行： `stage-1`，`react`，`es2015`。

###  插件参数
插件和 preset都可以接收参数，参数由插件名和参数对象组成一个数组，可以在配置文件中设置。
```
{
  "plugins": [
    [
      "transform-async-to-module-method",
      {
        "module": "bluebird",
        "method": "coroutine"
      }
    ]
  ]
}
```
如果不指定参数，下面几种形式都是一样的：
```
{
  "plugins": ["pluginA", ["pluginA"], ["pluginA", {}]]
}
```
preset 设置参数的工作原理完全相同
```
{
  "presets": [
    [
      "env",
      {
        "loose": true,
        "modules": false
      }
    ]
  ]
}
```

##  预设（Presets）  
Presets 是一组插件的组合，可作为共享配置。

### 官方的 preset
* `@babel/preset-env`
* `@babel/preset-react`
* `@babel/preset-flow`
* `@babel/preset-typescript`

### 创建 preset
自定义preset，只需导出一份配置即可。
```
// 可以返回一个插件数组
module.exports = function() {
  return {
    plugins: [
      "pluginA",
      "pluginB",
      "pluginC",
    ]
  };
}

// preset 也可以包含其他 preset，以及带有参数的插件
module.exports = () => ({
  presets: [
    require("@babel/preset-env"),
  ],
  plugins: [
    [require("@babel/plugin-proposal-class-properties"), { loose: true }],
    require("@babel/plugin-proposal-object-rest-spread"),
  ],
});
```

### preset 路径
和插件一样，路径可以是 npm 包名，也可以是相对路径或绝对路径。

### preset 的简写
如果 preset 的名称前缀为 `babel-preset-`，则可以使用缩写。同样也适用于带有冠名的 preset。

### preset的排列顺序和参数指定同插件

## Options
###  Primary options
#### `cwd`
type: `String`

default: `process.cwd()`

程序 options中的所有路径将相对于该工作目录进行解析

## polyfill
babel包含了一个polyfill,这个 polyfill 包括了自定义的[regenerator runtime](https://github.com/facebook/regenerator/blob/master/packages/regenerator-runtime/runtime.js) 和 [core-js](https://github.com/zloirock/core-js)

polyfill模拟了一个完整的 ES2015+环境（不小于 Stage 4 提案），并且它将被用在一个应用程序当中而并非库/工具（当使用`babel-node`时，polyfill 会被自动加载）。

这意味着你可以使用如`Promise`，`WeakMap`这样的内置对象；`Array.from`，`Object.assign`这样的静态方法；`Array.prototype.includes`这样的实例方法；generator functions(使用`regenerator`插件提供)。为了做到这些，polyfill 将这些添加到全局作用域和像 `String` 这样的原生原型上。

###  Installation
```
npm install --save @babel/polyfill
```

> 因为这是 polyfill（在源代码运行之前运行），需要作为`dependency`而不是`devDependency`

###  Size
polyfill 使用是很方便的，但你应该和`@babel/preset-env`，`useBuiltIns option`一起使用，以便于不要把整个 polyfill（有事并不需要）都包括进去。或者我们建议你手动的导入单独的 polyfill。

####  TC39 Proposals
如果你使用一个不是 Stage 4 阶段的提案，`@babel/polyfill`将不会为你自动导入这些。你必须单独地从其他像`core-js`这样的polyfil导入这些。我们可能会尽快将其作为单独的文件包含在`@babel/polyfill`中。

###  Usage in Node/Browserify/Webpack
在你的应用的`entry point`的顶部加载你所需要的 polyfill。
> 确保它在其他的代码或者 require语句之前被调用。
```
// commonJS Module
require('@babel/polyfill')

// ES Module
import '@babel/polyfill'
```

在 webpack 中，有多种方式包含 polyfill：
* 当和`@babel/preset-env`一起使用时
  * 如果在`.babelrc`中指定了`useBuildIns: 'usage'`,那么不要在`webpack.config.js`的 `entry array`和源码中包含`@babel/polyfill`。注意：此时你仍然需要安装`@babel/polyfill`。
  * 如果在`.babelrc`中指定了`useBuildIns: 'entry'`,那么然后在应用的`entry point`的顶部通过`require/import`包含`@babel/polyfill`。
  * 如果没有在`.babelrc`中指定了`useBuildIns`或者被明确的设置为`useBuildIns: false`,那么在`webpack.config.js`文件的 entry array中直接添加`@babel/polyfill`。
```
module.exports = {
  entry: ["@babel/polyfill", "./app/js"],
};
```
* 如果没有使用`@babel/preset-env`，那么像上面讨论中那样，直接把`@babel/polyfill`添加到webpack 的 entry array 中。它将仍然被通过`require/import`的方式添加到应用`entry point`的顶部，但不建议这样做。
> 不建议直接引入整个 polyfill，反之应该尝试`useBuildIns option`或者手动引入你需要的 polyfill（无论是从这个包还是别的什么地方）。

###  Usage in Browser
来自于`@babel/polyfill`的 npm 发布包的`dist/polyfill.js`是可用的。它需要在你所有的已编译 babel 代码之前被包含进来。你也可以将它添加到已编译代码的前面或者通过`script`标签引入。

**注意：**不要通过browserify等引入，使用`@babel/polyfill`。

###  Detail
> 如果你正在寻找能在工具/库 中使用，且不修改 `globals`，请查看[transform-runtime](https://www.babeljs.cn/docs/babel-plugin-transform-runtime)。这意味着您将无法使用上面提到的实例方法，例如`Array.prototype.includes`。

注意：根据您实际使用的ES2015方法，您可能不需要使用`@babel/polyfill`或运行时插件。 您可能只想加载正在使用的特定polyfill（例如`Object.assign`），或者仅记录正在加载库的环境应包含某些polyfill的文档（or just document that the environment the library is being loaded in should include certain polyfills）。


##  `@babel/plugin-transform-runtime`
### installation
```
npm install --save-dev @babel/plugin-transform-runtime

npm install --save @babel/runtime
```

转换插件只能在开发环境下使用，但运行时将被你发布的代码所依赖，所以需要在生产环境安装。下面的例子将提供更多的细节。

### Why?

### Usage

####  通过`.babelrc`（建议）
把下面的内容添加到`.babelrc`

without options:
```
{ 
  "plugins": ["@babel/plugin-transform-runtime"]
}
```

With options (and their defaults):
```
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "absoluteRuntime": false,
        "corejs": false,
        "helpers": true,
        "regenerator": true,
        "useESModules": false
      }
    ]
  ]
}
```
####  通过 `CLI`
```
babel --plugins @babel/plugin-transform-runtime script.js
```
####  通过 Node API
```
require("@babel/core").transform("code", {
  plugins: ["@babel/plugin-transform-runtime"],
});
```

### Options
####  `corejs`
`String`或`Number`，默认`false`

eg. `['@babel/plugin-transform-runtime',{corejs: 2}]`