##  nvm使用记录

使用nvm安装node
```
nvm install <version>

// nvm install 10.16.3
```

查看本地安装的node
```
nvm ls
```

给node起别名
```
nvm alias <name> <version>

// nvm alias default 10.16.3
// nvm alias next 12.11.0
```

切换node版本
```
nvm use <version | alias>

// 使用版本号切换
nvm use 10.16.3

// 使用别名切换
nvm use default
```