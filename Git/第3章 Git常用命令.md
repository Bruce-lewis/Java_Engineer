
| 命令名称                              | 作用      |
| --------------------------------- | ------- |
| git config --global user.name 用户名 | 设置用户签名  |
| git config --global user.email 邮箱 | 设置用户签名  |
| git config --global user.name     | 查看用户签名  |
| git init                          | 初始化本地库  |
| git status                        | 查看本地库状态 |
| git add 文件名                       | 添加到暂存区  |
| git commit -m "日志信息" 文件名          | 提交到本地库  |
| git reflog                        | 查看历史记录  |
| git reset --hard 版本号              | 版本穿梭    |
|                                   |         |

# 3.1 设置用户签名
git config --global user.name 用户名
git config --global user.email 邮箱

![[Pasted image 20250311204357.png]]


# 3.2 初始化本地库
i. 基本语法
git init 

ii. 实操案例
![[Pasted image 20250311204905.png]]

# 3.3 查看本地库状态
git status
![[Pasted image 20250311205114.png]]

新建一个hello.txt 文件
![[Pasted image 20250311205322.png]]

# 3.4 添加暂存区

git add hello.txt

再次查看
![[Pasted image 20250311205613.png]]

# 3.5 提交本地库
git commit -m "提交日志" 文件名

![[Pasted image 20250311205848.png]]


# 3.6 修改文件（hello.txt）
![[Pasted image 20250311210231.png]]

![[Pasted image 20250311210336.png]]

# 3.7 历史版本
## 3.7.1 查看历史版本

git reflog  查看版本信息
git log 查看版本详情信息
![[Pasted image 20250311210617.png]]

## 3.7.2 版本穿梭
git reset --hard 版本号
![[Pasted image 20250311210801.png]]

![[Pasted image 20250311211100.png]]