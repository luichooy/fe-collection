# 如何在Windows系统中配置命令别名

## PowerShell

#### 第一步、打开PowerShell 的配置文件
可以通过一下命令找到PowerShell 的配置文件在哪里：
```shell
echo $profile
```
比如输入如下文件路径
```shell
C:\Users\{username}\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

#### 第二步、打开这个文件添加alias配置
```ps1
function cdcode { cd E:\code }
function gck { git checkout }
function gcb { git checkout -b }
function ga { git add }
function gaa { git add . }
function gcm { git commit -m }
function gp { git push }
function gpf { git push -f }
function gpll { git pull }
function gb { git branch }
function gr { git reset }
function gt { git tag }
function master { git checkout master }
function dev { git checkout dev }
function develop { git checkout develop }
function pn { pnpm }
function pconfig { code C:\Users\{username}\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 }
function root { cd C:\Users\{username} }
function lsroot { ls C:\Users\{username}}
```

## 参考资料
[为 Windows PowerShell 设置 User Alias （命令别名）](https://zhuanlan.zhihu.com/p/74881435)
