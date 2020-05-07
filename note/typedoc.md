## Options

TypeDoc 接收大多数 TypeScript compiler 接收的命令行参数，所有未使用标志传递的参数都会被解析为输入文件。TypeDoc 也接收目录作为输入文件。命令行中传递的任何选项都会覆盖配置文件中的选项。

```
$ typedoc --out path/to/documentation/ path/to/typescript/project/
```

### Configuration Options

这些选项控制 TypeDoc 从何处读取它的配置

#### options

```
$ typedoc --options <filename>
```

指定要被加载的 json 配置文件，如果没有指定，TypeDoc 将从当前目录下查找 `typedoc.json` 或者 `typedoc.js`。JSON 文件应该返回 key 是选项名称组成的对象，例如：

```
{
  "inputFiles": ["./src"],
  "mode": "modules",
  "out": "doc"
}
```

#### tsconfig

```
$ typedoc --tsconfig </path/to/tsconfig.json>
```

指定从中读取配置选项的 `tsconfig.json` 文件。如果未指定，TypeDoc 会像 tsc 那样查找当前目录和父目录下的 `tsconfig.json` 文件

当 TypeDoc 加载 `tsconfig.json` 文件时，它将读取 `typedocOptions`key 中声明的选项，并且使用`tsconfig.json`的输入文件（使用`files`或者`include/exclude`定义）作为自己的输入文件。

### Input Options

这些选项控制 TypeDoc 处理那些文件来生成文档以及这些文件被如何处理

#### inputFiles

```
$ typedoc a b
# or
$ typedoc --inputFiles a --inputFiles b
```

指定要传递给 TypeScript 的初始输入文件。命令行上的`--inputFiles`说明符是可选的。 没有相应选项名称的任何参数都将被解析为输入文件。 此选项还可以接受目录，该目录将以递归方式扩展，并将所有文件添加到输入文件中。

#### mode

```
$ typedoc --mode <file|modules>
```

指定应如何转换项目。

`--mode file`

将所有输入文件视为全局文件，仅应用于在 tsconfig.json 中将模块设置为 none 的项目。

`--mode modules`

将项目中的每个文件记录为自己的模块，这对于生成内部使用文档的项目最有用。

`--mode library`

将每个扩展开的的输入文件记录为一个库的入口，从而将该文件的所有导出解析为属于该库。
如果将目录指定为输入文件，则该目录和子目录中的所有文件都将被视为入口。
