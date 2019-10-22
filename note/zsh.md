## 相关命令
查看系统内置了哪些shell
```
cat /etc/shells

# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```
查看当前系统使用的shell
```
echo $0
```
切换系统默认shell
```
# 切换至zsh
chsh -s /bin/zsh

# 切换至bash
chsh -s /bin/bash
```
注意：
* 切换的时候，需要你输入当前系统用户的密码
* 切换之后，需要关掉当前shell窗口，重新打开之后才会生效

###  下载oh-my-zsh
* 手动下载(需安装Git)
```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```
* 自动下载（需安装wget）
```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

## 配置前置知识
安装完成就需要对oh-my-zsh进行配置了，oh-my-zsh的配置文件是 `~/.zshrc`

* 使用vim打开配置文件
```
vim ~/.zshrc
```
但是这种方式修改太麻烦了，我们可以使用编辑器打开这个文件进行修改
* 使用vscode打开
```
code ~/.zshrc
```
如果code命令不可用，打开vscode,按下`shift + command + p`,选择出现的下拉菜单中的“在PATH中安装‘code’命令”，安装完成就可以在shell中使用`code`命令了。

注意： ***对`~/.zshrc`文件修改之后，必须重启才能生效***，重启命令：
```
source ~/.zshrc
```
***并且shell工具（item2）也要重启***

## 配置oh-my-zsh

####  配置别名
别名就是命令行中使用的命令的别名，我们可以给一些常用的命令配置别名，避免每次都输入很长的命令。别名的配置在配置文件最底部给出了配置例子
```
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
```
可以看到，配置的基本格式就是`alias <别名>="<命令>"`，现在我们在例子后面添加两条自己的配置

```
alias ll="ls -l"
alias la="ls -a"
```
这样我们每次需要使用`ls -l`或`ls -a`的时候，只需要在命令行中输入`ll`或`la`就可以了


####  配置主题
在配置文件`~/.zshrc`中找到`ZSH_THEME="robbyrussell"`,默认主题是`robbyrussell`，此时只需要将`robbyrussell`替换成你想要的主题就可以了。

可供选择的主题及其效果，在 https://github.com/robbyrussell/oh-my-zsh/wiki/Themes 可以查看。

##  关于nvm
安装oh-my-zsh之后，在命令行中使用nvm的时候，发现nvm命令无法使用了,这是因为nvm安装之后的路径配置默认存放在bash的配置文件中，而我们现在使用的是zsh，所以需要针对zsh进行配置。

使用vscode打开bash的配置文件`~/.bash_profile`
```
code ~/.bash_profile
```
将文件中的如下内容复制到`~/.zshrc`中，然后重启`~/.zshrc`.
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

https://www.jianshu.com/p/a5f478a143dc
