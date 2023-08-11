# SSH keys

当使用github、gitlab等仓库时，为了不输入密码，我们可以使用ssh url克隆项目到本地，这样以后提交代码的时候就不需要输入用户名密码了。

> 使用 ssh url 克隆项目之前，需要先在对应的代码托管平台配置好 ssh keys

## 生成ssh keys

在对应的代码托管平台配置ssh keys之前，我们要先在本地生成ssh keys。生成ssh keys的步骤如下：

#### 第一步、首先检查本地是否已经存在ssh keys

有可能你曾经已经生成过ssh keys了，此时就可以直接使用已经存在的ssh keys了，没必要重新生成，当然你也可以选择重新生成。

检查方法为：

MacOS下：
```shell
ls -al ~/.ssh 
```

Windows系统：检查 `C:\users\{username}`目录下是否存在.ssh目录，这里的username指的是你当前登录的用户名。通常windows下的大多数软件配置目录都是这个，相当于MacOS下的`~`目录

#### 第二步、生成 ssh keys

如果通过第一步确定本地不存在ssh keys，则接下来需要再本地生成ssh keys

生成方法如下：

```shell
ssh-keygen -t rsa -C "注释文字"
```

代码参数含义：

* -t 制定秘钥类型，默认为rsa，可以省略
* -C 注释文字，比如可以是你的邮箱等
* -f 制定秘钥文件存储文件名

在控制台输入以上命令后，出现如下内容：

```shell
ssh-keygen -t rsa -C 'xxx@gmail.com'
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\{username}/.ssh/id_rsa):
Created directory 'C:\\Users\\{username}/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\{username}/.ssh/id_rsa
Your public key has been saved in C:\Users\{username}/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:IN/4K7Id2AJXXojYiyFM4ZI/SjwmT13PNFImiWOCFfo xxx@gmail.com
The key's randomart image is:
+---[RSA 3072]----+
| =+. ...o        |
|=o.o+..=         |
|=ooo+.* +        |
|o+ + B X .       |
|.=E + + S        |
|++.+ o .         |
|. . o o .        |
|    .o.. .       |
|    .o...        |
+----[SHA256]-----+
```

如上所示，输入命令后，会以此提示你输入 ssh keys保存位置、重复两次密码。这两个一般都是直接按回车键试用默认值。其中：

* 默认保存位置是：
    * MacOS: `~`
    * Windows: `C:\Users\{username}`
* 默认密码是无密码，无密码的情况下当提交代码时不需要输入密码，有密码的情况下提交代码需要输入这个密码。因此最好不要设置，不然忘记了就要把这里所有的流程重新走一遍。

## ssh keys保存位置
以上都默认的情况下，默认会在用户根目录（也就是上面的默认保存位置）下创建一个`.ssh`目录，并在该目录下生成以下两个文件：
* `id_rsa` 私钥文件
* `id_rsa.pub` 公钥文件

我们需要把`id_rsa.pub`公钥文件中的内容复制到对应的代码托管平台的输入框中去。具体粘贴到哪里视平台而定。通常来说都有一个“SSH and GPG keys”的管理页面，在这个页面就可以找到相应的ssh keys 输入框。如以下为github的配置页面：

[SSH and GPG keys](https://github.com/settings/keys)

## 参考资料
[Github 生成SSH秘钥（详细教程）](https://www.cnblogs.com/yuqiliu/p/12551258.html)
