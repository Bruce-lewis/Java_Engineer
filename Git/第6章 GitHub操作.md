Github 网址：https://github.com

# 6.1 创建远程仓库
![[Pasted image 20250311215931.png]]
![[Pasted image 20250311215940.png]]

# 6.2 远程仓库操作

| 命令名称                             | 作用                            |
| -------------------------------- | ----------------------------- |
| git remote -v                    | 查看当前所有远程地址别名                  |
| git remote add 别名 远程地址           | 起别名                           |
| git push 别名 分支                   | 推送本地                          |
| git clone 远程地址                   | 将远程仓库的内容克隆到本地                 |
| git pull 远程库地址别名 远程分支名           | 将远程仓库对于分支最新内容拉下来之后与当前本地分支直接合并 |
| git fetch 远程库地址别名                | 用于从远程仓库下                      |
| git remote set-url origin 新的仓库地址 | 修改远程仓库地址                      |
|                                  |                               |
## 6.2.1 创建远程库别名
![[Pasted image 20250312092904.png]]
## 6.2.2 推送本地库到远程库
![[Pasted image 20250312093453.png]]

## 6.2.3 拉取远程库到本地库
![[Pasted image 20250312093939.png]]
## 6.2.4 克隆远程库到本地
克隆不需要登录账号，有git工具就行（私有库必须要权限）

令狐冲clone代码
![[Pasted image 20250312094229.png]]


## 6.2.5 团队内协作
令狐冲提交代码，push远程
![[Pasted image 20250312094557.png]]
push推送远程库，需要登录远程库的账号
![[Pasted image 20250312094740.png]]
登陆之后，提示 该项目 没有权限（岳不群没有授权）
![[Pasted image 20250312094925.png]]

岳不群在github的 git-demo 项目中，将令狐冲加入到团队中，此时会生成一个邀请函。

然后令狐冲需要 接受这个邀请函。

然后令狐冲就可以推送成功了
![[Pasted image 20250312095446.png]]

# 6.3 跨团队协作
东方不败 去 fork 岳不群的 华山剑法
![[Pasted image 20250312095756.png]]

东方不败修改了代码后

创建一个拉取请求
![[Pasted image 20250312100122.png]]


![[Pasted image 20250312100202.png]]

然后岳不群就在自己的账号下看见了Pull request。
![[Pasted image 20250312100235.png]]
然后岳不群就可以审查，合并了
![[Pasted image 20250312100708.png]]

# 6.4 SSH免密登录
![[Pasted image 20250312100946.png]]

具体操作如下：
![[Pasted image 20250312101018.png]]

生成了公钥和私钥后，
进入到岳不群的 远程仓库github中
![[Pasted image 20250312101248.png]]
![[Pasted image 20250312101324.png]]

然后岳不群就可以通过 ssh 链接 拉取pull和推送push代码了
![[Pasted image 20250312101529.png]]