![[Pasted image 20250311211344.png]]
# 4.1 什么是分支
![[Pasted image 20250311211441.png]]
![[Pasted image 20250311211713.png]]

# 4.2 分支的好处
同时并行推进多个功能开发，提高开发效率。
各个分支在开发过程中，如果某一个分支开发失败，不会对其它分支有任何影响。失败的分支删除重新开始即可。
# 4.3 分支的操作

| 命令名称             | 作用           |
| ---------------- | ------------ |
| git branch 分支名   | 创建分支         |
| git branch -v    | 查看分支         |
| git checkout 分支名 | 切换分支         |
| git merge 分支名    | 把指定分支合并到当前分支 |
|                  |              |
![[Pasted image 20250311212326.png]]


![[Pasted image 20250311212401.png]]

![[Pasted image 20250311212459.png]]

## 正常合并
合并没有冲突时，
因为hot-fix分支的修改，但是master分支没有修改
![[Pasted image 20250311212629.png]]

## 冲突合并
![[Pasted image 20250311212851.png]]

![[Pasted image 20250311213041.png]]

![[Pasted image 20250311213159.png]]

![[Pasted image 20250311213310.png]]

![[Pasted image 20250311213323.png]]

### 1. 解决冲突
### 2. 添加到暂存区
### 3. 执行提交（注意：此时不能带文件名）
![[Pasted image 20250311213545.png]]

![[Pasted image 20250311213834.png]]
![[Pasted image 20250311213715.png]]