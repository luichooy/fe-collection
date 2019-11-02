# 配置
有两种方式来配置 eslint
1.  使用 js 注释直接把配置信息嵌入到代码源文件中
1.  使用 JavaScript，JSON 或 YAML 文件为整个目录和它的子目录指定配置信息

使用第二种方式指定配置信息的方式有：
* 在`.eslintrc.*`中配置
* 在 `package.json` 中的`eslintConfig`字段指定配置信息

注意： **如果你的主目录（通常 ~/）存在一个配置文件，eslint 只有在找不到其他配置文件时才使用它**

##  `parseOptions`
解析器选项用来指定你想要支持的 `JavaScript` 语言选项，设置解析器选项能帮助 ESLint 确定什么是解析错误。eslint 默认支持 `ECMAScript 5` 语法，你可以覆盖该设置，以启用对 `ECMAScript` 其它版本和 `JSX` 的支持。

注意： **支持 JSX 并不意味着支持 React，支持 es6语法并不意味着同时支持 es6新的全局变量或类型**

可用的选项有：
* `ecmaVersion`: 默认为3，5，可以设置为 6（2015），7（2016），8（2017），9（2018），10（2019）
* `sourceType`: 默认`"script"`，如果你使用 `ECMAScript` 模块，可以设置为`"module"`,
* `ecmaFeatures`: 对象，标识你想使用的额外的语言特性：
  * `globalReturn`: 允许在全局作用域下使用`return`语句
  * `impliedStrict`: 启用全局`strict mode`(如果ecmaVersion是 5 或者更高)
  * `JSX`: 启用 JSX

```
// 示例：

{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": "error"
    }
}
```

##  `parser`
`parse`用来指定解析器，eslint默认的解析器是`Espree`,你可以通过该选项指定其他解析器。
```
{
    "parser": "esprima",
    "rules": {
        "semi": "error"
    }
}
```

##  `processor`
`processor`选项用来指定处理器。插件可以提供处理器。处理器可以从另一种文件中提取 JavaScript 代码，然后让 ESLint 检测 JavaScript 代码。或者处理器可以在预处理中转换 JavaScript 代码。

`process`的值由插件名和处理器名组成的串接字符串加上斜杠。例如，下面的选项启用插件 `a-plugin` 提供的处理器 `a-processor`：
```
{
    "plugins": ["a-plugin"],
    "processor": "a-plugin/a-processor"
}
```
要为特定类型的文件指定处理器，请使用 `overrides` 键和 `processor` 键的组合。例如，下面对 `*.md` 文件使用处理器 `a-plugin/markdown`
```
{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        }
    ]
}
```
处理器可以生成命名的代码块，如 `0.js` 和 `1.js`。`ESLint` 将这样的命名代码块作为原始文件的子文件处理。你可以在配置的 `overrides` 部分为已命名的代码块指定附加配置。例如，下面的命令对以 `.js` 结尾的 `markdown` 文件中的已命名代码块禁用 `strict` 规则：
```
{
    "plugins": ["a-plugin"],
    "overrides": [
        {
            "files": ["*.md"],
            "processor": "a-plugin/markdown"
        },
        {
            "files": ["**/*.md/*.js"],
            "rules": {
                "strict": "off"
            }
        }
    ]
}
```

##  `env`
`env`选项用来指定环境。一个环境定义了一组预定义的全局变量。可用的环境包括：
* `browser` - 浏览器环境中的全局变量。
* `node` - Node.js 全局变量和 Node.js 作用域。
* `commonjs` - CommonJS 全局变量和 CommonJS 作用域 (用于 Browserify/WebPack 打包的只在浏览器中运行的代码)。
* `shared-node-browser` - Node.js 和 Browser 通用全局变量。
* `es6` - 启用除了 modules 以外的所有 ECMAScript 6 特性（该选项会自动设置 ecmaVersion 解析器选项为 6）。
* `worker` - Web Workers 全局变量。
* `amd` - 将 require() 和 define() 定义为像 amd 一样的全局变量。
* `mocha` - 添加所有的 Mocha 测试全局变量。
* `jasmine` - 添加所有的 Jasmine 版本 1.3 和 2.0 的测试全局变量。
* `jest` - Jest 全局变量。
* `phantomjs` - PhantomJS 全局变量。
* `protractor` - Protractor 全局变量。
* `qunit` - QUnit 全局变量。
* `jquery` - jQuery 全局变量。
* `prototypejs` - Prototype.js 全局变量。
* `shelljs` - ShellJS 全局变量。
* `meteor` - Meteor 全局变量。
* `mongo` - MongoDB 全局变量。
* `applescript` - AppleScript 全局变量。
* `nashorn` - Java 8 Nashorn 全局变量。
* `serviceworker` - Service Worker 全局变量。
* `atomtest` - Atom 测试全局变量。
* `embertest` - Ember 测试全局变量。
* `webextensions` - WebExtensions 全局变量。
* `greasemonkey` - GreaseMonkey 全局变量。

这些环境并不是互斥的，所以你可以同时定义多个。

```
// 在 JavaScript 文件中使用注释来指定环境
/* eslint-env node, mocha */

// .eslintrc
{
    "env": {
        "browser": true,
        "node": true
    }
}
```

如果你想在一个特定的插件中使用一种环境，确保提前在 plugins 数组里指定了插件名，然后在 env 配置中不带前缀的插件名后跟一个 / ，紧随着环境名。例如：

```
{
    "plugins": ["example"],
    "env": {
        "example/custom": true
    }
}
```

##  `globals`
`globals`选项用来指定全局变量。当访问当前源文件内未定义的变量时，`no-undef` 规则将发出警告。如果你想在一个源文件里使用全局变量，推荐你在 ESLint 中定义这些全局变量，这样 ESLint 就不会发出警告了。

你可以使用注释或在配置文件中定义全局变量。

要在你的 JavaScript 文件中，用注释指定全局变量，格式如下：
```
/* global var1, var2 */
```
如果你想选择性地指定这些全局变量可以被写入(而不是只被读取)，那么你可以用一个 `"writable"` 的标志来设置它们:
```
/* global var1:writable, var2:writable */
```
在配置文件中指定
```
// .eslintrc
{
    "globals": {
        "var1": "writable",
        "var2": "readonly"
    }
}
```
可以使用字符串 "off" 禁用全局变量。例如，在大多数 ES2015 全局变量可用但 Promise 不可用的环境中，你可以使用以下配置:
```
{
    "env": {
        "es6": true
    },
    "globals": {
        "Promise": "off"
    }
}
```
**注意**：要启用no-global-assign规则来禁止对只读的全局变量进行修改。

##  `plugins`
`plugins`用来配置插件列表。ESLint 支持使用第三方插件。在使用插件之前，你必须使用 npm 安装它。

插件名称可以省略 `eslint-plugin-` 前缀。
```
{
    "plugins": [
        "plugin1",
        "eslint-plugin-plugin2"
    ]
}
```

##  `rules`
`rules`选项用来指定规则。

要改变一个规则设置，你必须将规则 ID 设置为下列值之一：
* `"off"` 或 `0` - 关闭规则
* `"warn"` 或 `1` - 开启规则，使用警告级别的错误：`warn` (不会导致程序退出)
* `"error"` 或 `2` - 开启规则，使用错误级别的错误：`error` (当被触发的时候，程序会退出)

####  使用注释配置规则
```
/* eslint eqeqeq: "off", curly: "error" */

/* eslint eqeqeq: 0, curly: 2 */

/* eslint quotes: ["error", "double"], curly: 2 */

/* eslint "plugin1/rule1": "error" */
```

####  使用配置文件配置规则
```
{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}
```
配置定义在插件中的一个规则的时候，你必须使用 插件名/规则ID 的形式。比如：
```
{
    "plugins": [
        "plugin1"
    ],
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"],
        "plugin1/rule1": "error"
    }
}
```
**注意**：当指定来自插件的规则时，确保删除 eslint-plugin- 前缀。ESLint 在内部只使用没有前缀的名称去定位规则。

##  `settings`
`settings`对象用来在配置文件中添加共享设置，这些共享设置将被提供给每一个规则。如果你想添加的自定义规则而且使它们可以访问到相同的信息，这将会很有用，并且很容易配置。
```
{
    "settings": {
        "sharedData": "Hello"
    }
}
```

##  eslint配置文件
####  eslint寻找配置文件过程
ESLint 将自动在要检测的文件目录里寻找它们，紧接着是父级目录，一直到文件系统的根目录（除非指定 root: true）
####  各种配置文件的优先级
* .eslintrc.js
* .eslintrc.yaml
* .eslintrc.yml
* .eslintrc.json
* .eslintrc（弃用）
* package.json

####  配置有层次结构（及多层目录结构中存在多个配置）时的优先级：
1.  行内配置
    1.  `/*eslint-disable*/` 和 `/*eslint-enable*/`
    1.  `/*global*/`
    1.  `/*eslint*/`
    1.  `/*eslint-env*/`
1.  命令行选项（或 CLIEngine 等价物）：
    1.  `--global`
    1.  `--rule`
    1.  `--env`
    1.  `-c、--config`
1.  项目级配置：
    1.  与要检测的文件在同一目录下的 `.eslintrc.*` 或 `package.json` 文件
    1.  继续在父级目录寻找 `.eslintrc` 或 `package.json`文件，直到根目录（包括根目录）或直到发现一个有`"root": true`的配置。
1.  如果不是（1）到（3）中的任何一种情况，退回到 `~/.eslintrc` 中自定义的默认配置。

##  `extends`
`extends`用来指定继承关系，其值可以是：
* 指定配置的字符串(配置文件的路径、可共享配置的名称、`eslint:recommended` 或 `eslint:all`)
* 字符串数组：每个配置继承它前面的配置

`extends` 属性值可以省略包名的前缀 `eslint-config-`。

####    使用 `"eslint:recommended"`
值为 "eslint:recommended" 的 extends 属性启用一系列核心规则，这些规则报告一些常见问题，在 规则页面 中被标记为`✅`

####    可共享的配置
可共享的配置 是一个 npm 包，它输出一个配置对象。要确保这个包安装在 ESLint 能请求到的目录下。
`extends` 属性值可以由以下组成：

*   `plugin:`
*   包名 (省略了前缀，比如，react)
*   `/`
*   配置名称 (比如 recommended)

例如：
```
"extends": [
    "eslint:recommended",
    "plugin:vue/essential"
],
```
####    使用配置文件
`extends`的属性值可以是一个指向基本配置文件的绝对或相对路径。eslint相对于该配置文件的位置来解析 `extends` 属性值中的相对路径。

```
{
    "extends": [
        "./node_modules/coding-standard/eslintDefaults.js",
        "./node_modules/coding-standard/.eslintrc-es6",
        "./node_modules/coding-standard/.eslintrc-jsx"
    ],
    "rules": {
        "eqeqeq": "warn"
    }
}
```
####    使用`"eslint:all"`
`extends`的属性值可以是`"eslint:all"`,该值会开启当前安装的 eslint 版本的所有core rules。

**注意：**不建议在生产环境使用该配置，因为它随 eslint 的主版本和此版本的变化而变化。

##  配置文件中的注释
在`JSON`和`YAML`中支持注释（不包括 `package.json` 文件）。你可以在这两种文件中使用 `JavaScript` 风格注释或 `YAML` 风格的注释。eslint将忽略这些注释，这允许你的 eslint 配置文件更加友好，例如：
```
{
    "env": {
        "browser": true
    },
    "rules": {
        // Override our default settings just for this directory
        "eqeqeq": "warn",
        "strict": "off"
    }
}
```
##  忽略文件或目录
你可以通过在项目根目录创建`.eslintignore`文件来告诉 eslint忽略(不检查)某些特定的文件或目录。`.eslintignore`文件是一个文本文件，它的每一行都是一个全局模式，该模式指示哪个路径应该被排除（即不检查）到 eslint 的检查之外，例如下面的例子将排除掉所有的 JavaScript 文件：
```
**/*.js
```
当 eslint运行时，它会在决定哪些文件需要被 lint之前，去当前工作目录下查找`.eslintignore`文件，如果找到文件，那么当 eslint 遍历目录时，`.eslintignore`文件中提及的路径都会被忽略，当前工作目录下只能存在一个`.eslintignore`文件，超过一个将不会被使用。

Globs are matched using node-ignore, so a number of features are available:
*   以 `#` 开头的行被当作注释，不影响忽略模式。
*   路径是相对于 `.eslintignore` 的位置或当前工作目录。通过 `--ignore-pattern` command 传递的路径也是如此。
*   忽略模式同 `.gitignore` 规范。
*   以 `!` 开头的行是否定模式，它将会重新包含一个之前被忽略的模式。
*   忽略模式依照 .gitignore 规范。


#  命令行

##  install

使用 eslint 需首先安装npm，安装 npm 之后安装 eslint
```
npm install eslint --save-dev
```
安装好之后以如下命令运行 eslint
```
eslint [options] [file|dir|glob]*
```
比如：
```
eslint file1.js file2.js

eslint lib/**
```

##  options

### 基本配置

| 配置项  | 含义 | 默认| 示例 |
| :-----:|  :----|:----:  |:---|
| `--no-eslintrc`  |禁用 .eslintrc.* 和 package.json 文件中的配置 ||`eslint --no-eslintrc file.js`
| `-c, --config`   | 为 ESLint (查看 Configuring ESLint 了解更多)指定一个额外的配置文件 ||`eslint -c ~/my-eslint.json file.js` |
|`--env`|指定环境||`eslint --env browser --env node file.js`|
|`--ext`|指定 ESLint 在指定的目录下查找 JavaScript 文件时要使用的文件扩展名|`.js`|`eslint . --ext .js,.js2`|
|`--global`|定义全局变量,这样它们就不会被 no-undef 规则标记为未定义了。任何指定的全局变量默认是只读的，在变量名字后加上 :true 后会使它变为可写||`eslint --global require,exports:true file.js`|
|`--parser`|为 ESLint 指定一个解析器|espree||
|`--parser-options`|指定 ESLint 要使用的解析器选项,可用的解析器选项取决于你所选用的解析器|||
|`--resolve-plugins-relative-to`|更改插件解析所在的文件夹|||

### 指定 rules 和 plugins

| 配置项  | 含义 |  示例 |
| :-----:|  :----|:---|
| `--rulesdir`  |||
| `--plugin`   | 这个选项指定一个要加载的插件。你可以省略插件名的前缀 `eslint-plugin-` |`eslint --plugin eslint-plugin-mocha file.js` |
|`--rule`|这个选项指定要使用的规则。这些规则将会与配制文件中指定的规则合并|`eslint --rule 'quotes: [2, double]'`|

### Fixing Problems

#### `--fix`

该选项指示 ESLint 试图修复尽可能多的问题。修复只针对实际文件本身，而且剩下的未修复的问题才会输出。不是所有的问题都能使用这个选项进行修复，该选项在以下情形中不起作用：
1.  当代码传递给 ESLint 时，这个选项抛出一个错误。
1.  该选项对使用处理器的代码没有影响，除非处理器选择允许自动修复。

如果你想从 `stdin` 修复代码或希望在不实际写入到文件的情况下进行修复，使用 `--fix-dry-run` 选项。

#### --fix-dry-run

该选项与 --fix 有相同的效果，唯一一点不同是，修复不会保存到文件系统中。

#### --fix-type

此选项允许你在使用 --fix 或 --fix-dry-run 时指定要应用的修复的类型。修复的三种类型是:
1.  `problem` - 修复代码中的潜在错误
1.  `suggestion` - 对改进它的代码应用修复
1.  `layout` - 应用不改变程序结构 (AST) 的修复

```
eslint --fix --fix-type suggestion,layout .
```

### 忽略文件

| 配置项  | 含义 | 默认| 示例 |
| :-----:|  :----|:----:  |:---|
| `--ignore-path`  |指定一个文件作为`.eslintignore`|`.eslintignore`|`eslint --ignore-path tmp/.eslintignore file.js`|
| `--no-ignore`   | 禁止排除 `.eslintignore`、`--ignore-path` 和 `--ignore-pattern` 文件中指定的文件 ||`eslint --no-ignore file.js` |
|`--ignore-pattern`|指定要忽略的文件模式(除了 .eslintignore 中的模式之外)||`eslint --ignore-pattern '/lib/' --ignore-pattern '/src/vendor/*' .`|

### 使用 stdin

| 配置项  | 含义 |  示例 |
| :-----:|  :---- |:---|
| `--stdin`  |告诉 ESLint 从 STDIN 而不是从文件中读取和检测源码|`cat myfile.js | eslint --stdin`|
| `--stdin-filename`   | 指定一个文件名去处理 STDIN |`cat myfile.js | eslint --stdin --stdin-filename=myfile.js` |

### 处理警告

| 配置项  | 含义 | 示例 |
| :-----:|  :----|:---|
| `--quiet`  |忽略警告，如果开启这个选项，ESLint 只会报告错误|`eslint --quiet file.js`|
| `--max-warnings`   | 指定一个警告的阈值，当你的项目中有太多违反规则的警告时，这个阈值被用来强制 ESLint 以错误状态退出 |`eslint --max-warnings 10 file.js` |

### 输出

| 配置项  | 含义 | 默认| 示例 |
| :-----:|  :----|:----:  |:---|
| `-o, --output-file`  |将报告写到一个文件||`eslint -o ./test/test.html`|
| `-f, --format`   | 指定控制台的输出格式 |`stylish`|`eslint -f compact file.js` |
| `--color, --no-color`  |强制启用/禁用彩色输出||`eslint --color file.js | cat`|

### Inline configuration comments

| 配置项  | 含义 | 示例 |
| :-----:|  :----|:---|
| `--no-inline-config`  |这个选项会阻止像 `/*eslint-disable*/` 或者 `/*global foo*/` 这样的内联注释起作用|`eslint --no-inline-config file.js`|
| `--report-unused-disable-directives`   |  | |

### 缓存

#### --cache

存储处理过的文件的信息以便只对有改变的文件进行操作。

缓存默认被存储在 `.eslintcache`。

启用这个选项可以显著改善 ESLint 的运行时间，确保只对有改变的文件进行检测

**如果你运行 ESLint --cache，然后又运行 ESLint 不带 --cache，.eslintcache 文件将被删除**

**自动修复的文件不放在缓存中。不触发自动修复的后续检测将把它放在缓存中**

#### --cache-location

缓存文件的路径。可以是一个文件或者一个目录

如果没有指定，则使用 `.eslintcache`

这个文件会在 eslint 命令行被执行的文件目录中被创建。

如果指定一个目录，缓存文件将在指定的文件夹下被创建。文件名将基于当前工作目录（CWD) 的 hash 值，比如：`.cache_hashOfCWD`

**如果不存在缓存文件的目录，请确保在尾部添加 /（*nix 系统）或 \（windows 系统）。否则该路径将被假定为是一个文件**

### 其他

| 配置项  | 含义 | 
| :-----:|  :----|
| `--init`  |这个选项将会配置初始化向导。它被用来帮助新用户快速地创建 `.eslintrc` |
| `--debug`   | 这个选项将调试信息输出到控制台 |
| `-h, --help`  |输出帮助菜单，显示所有可用的选项|
| `-v, --version`  |输出当前 ESlint 的版本|
| `--print-config`  |输出传递的文件使用的配置|

##  Ignoring files from linting

eslint使用`.eslintigonre`文件来避免检测处理,`.eslintignore` 文件是个纯文本文件，每一行都包含一种模式。

它可以放在目标目录的任何父级目录；它将影响到它所在的当前目录和所有子目录。

```
node_modules/*
**/vendor/*.js
```

##  退出码

当检测文件时，ESLint 将使用以下退出代码之一退出:
* `0`: 检测成功，没有错误。如果 `--max-warnings` 标志被设置为 n，那么警告数量最多为n。
* `1`: 检测成功，并且至少有一个错误，或者警告多于 --max-warnings 选项所允许的警告。
* `2`: 由于配置问题或内部错误，检测未能成功。


