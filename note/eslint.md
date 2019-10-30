##  命令行

####  install

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

####  options

**基本配置**
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

**指定 rules 和 plugins**
| 配置项  | 含义 | 默认| 示例 |
| :-----:|  :----|:----:  |:---|
| `--rulesdir`  ||||
| `--plugin`   | 这个选项指定一个要加载的插件。你可以省略插件名的前缀 `eslint-plugin-` ||`eslint --plugin eslint-plugin-mocha file.js` |
|`--rule`|这个选项指定要使用的规则。这些规则将会与配制文件中指定的规则合并||`eslint --rule 'quotes: [2, double]'`|

**Fixing Problems**

```
--fix
```

该选项指示 ESLint 试图修复尽可能多的问题。修复只针对实际文件本身，而且剩下的未修复的问题才会输出。不是所有的问题都能使用这个选项进行修复，该选项在以下情形中不起作用：
1.  当代码传递给 ESLint 时，这个选项抛出一个错误。
1.  该选项对使用处理器的代码没有影响，除非处理器选择允许自动修复。

如果你想从 `stdin` 修复代码或希望在不实际写入到文件的情况下进行修复，使用 `--fix-dry-run` 选项。

```
--fix-dry-run
```
该选项与 --fix 有相同的效果，唯一一点不同是，修复不会保存到文件系统中。

```
--fix-type
```

此选项允许你在使用 --fix 或 --fix-dry-run 时指定要应用的修复的类型。修复的三种类型是:
1.  `problem` - 修复代码中的潜在错误
1.  `suggestion` - 对改进它的代码应用修复
1.  `layout` - 应用不改变程序结构 (AST) 的修复

```
eslint --fix --fix-type suggestion,layout .
```

**忽略文件**

| 配置项  | 含义 | 默认| 示例 |
| :-----:|  :----|:----:  |:---|
| `--ignore-path`  |指定一个文件作为`.eslintignore`|`.eslintignore`|`eslint --ignore-path tmp/.eslintignore file.js`|
| `--no-ignore`   | 禁止排除 `.eslintignore`、`--ignore-path` 和 `--ignore-pattern` 文件中指定的文件 ||`eslint --no-ignore file.js` |
|`--ignore-pattern`|指定要忽略的文件模式(除了 .eslintignore 中的模式之外)||`eslint --ignore-pattern '/lib/' --ignore-pattern '/src/vendor/*' .`|


