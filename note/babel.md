babel 就是一个编译器（输入源码 -> 输出编译后的代码），就像其他编译器一样，编译过程分为三个阶段： 解析，转换和打印输出。

babel 虽然开箱即用，但他什么都不做，类似于`const babel = code => code`，即将代码解析之后，再输出同样的代码。如果想让 babel 做一些事情，就需要为其制定插件。

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

