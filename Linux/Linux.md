# 1. Linux基础命令
## ls  文件查看

ls -ahlp
1. **`-a`**：显示所有文件，包括以点（`.`）开头的隐藏文件。
2. `-h`：与 `-l` 选项一起使用时，以人类可读的格式显示文件大小（例如，K、M、G）。
3. -l: 以列表的形式展示文件信息，包含文件的详细信息
4. -p： 追加 / 到文件目录，区分文件夹和文件（防止有些系统的文件夹不带颜色）

ls -lR
1.  -R: 递归列举文件夹下的文件


## cd & pwd 目录切换
cd 是进入一个目录。

下面说一些细节
但是我们直接输入
```bash
##直接进入用户根目录
cd

#进入上一次目录
cd -

#进入上一层目录
cd ..
```

## mkdir 目录创建
```shell
# make parent directories as needed
mkdir -p myparentdir/mydir
```
## chmod 修改权限
```shell
# 
chmod -R 777 /mydir
```


![[Pasted image 20250109135103.png]]
![[Pasted image 20250109135135.png]]
## chown 修改所有者
![[Pasted image 20250109135248.png]]


## rm 删除文件夹或者文件
```shell
rm -rf dir[/file]
```
1. -r:  recursively 递归删除一个文件夹
2. -f:  force 强制删除

## ==通配符==
```shell
# 删除以test开头的文件 
rm -rf test* 
```

## 文件操作命令
### touch 

### cp
```shell
# 递归复制
cp -r [path]/source_file [path]/destination_file
```

### mv
移动或者改名


### vim
编辑文件
![[Pasted image 20250109133441.png]]
![[Pasted image 20250109133421.png]]
![[Pasted image 20250109134244.png]]
![[Pasted image 20250109134320.png]]
![[Pasted image 20250109134356.png]]
### cat

### more
对于大文件，翻页查看

空格翻页
### tail 
![[Pasted image 20250109133109.png]]

## find 查找文件
```shell
#指定文字查找
find /home/data -name "file_name.txt"

#模糊查找
find /home/data -name "file*"

#按照文件大小查文件
语法：find 起始路径 -size +|-n[kMG]
查找大于10M的文件
find / -size +10M
```

## grep 文件中搜索特定
```shell
在文件 `file.txt` 中搜索字符串 "hello"：
grep "hello" file.txt

忽略大小写搜索
grep -i "hello" file.txt

在当前目录及其子目录中查找字符串：
grep -r "要查找的字符串" .

递归搜索当前目录及其子目录中的所有 `.txt` 文件：
grep -r "hello" *.txt

显示匹配行的行号：
grep -n "hello" file.txt


grep -v "hello" file.txt
```

## ==管道符==
将左边命令的结果 作为 右边命令的输入
```shell
ps aux | grep "xxxx"
```

## wc 文件内容统计
![[Pasted image 20250109131627.png]]
```shell
wc test.txt
```

查看文件夹中的文件数量
```shell
ls -l /usr/bin | wc -l
```

## echo 
![[Pasted image 20250109132851.png]]
![[Pasted image 20250109132907.png]]

## 重定向符
![[Pasted image 20250109133017.png]]




# 2. 实用操作
## 2.1 常用快捷键
### ctrl+c 结束命令或者交互进程

![[Pasted image 20250109141137.png]]
### ctrl +d 
![[Pasted image 20250109141208.png]]

### history 查看历史命令
! no 执行某个历史命令

![[Pasted image 20250109141532.png]]

![[Pasted image 20250109141707.png]]

## 2.2 软件安装
![[Pasted image 20250109142037.png]]
![[Pasted image 20250109142124.png]]
![[Pasted image 20250109142326.png]]
![[Pasted image 20250109142529.png]]

## 2.3 systemctl 
![[Pasted image 20250109142711.png]]

## 2.4 ln 软连接
![[Pasted image 20250109143237.png]]

## 2.5 日期和时区
![[Pasted image 20250109143554.png]]
![[Pasted image 20250109143634.png]]
![[Pasted image 20250109143754.png]]

## 2.6 网络
![[Pasted image 20250109150608.png]]
![[Pasted image 20250109150654.png]]
![[Pasted image 20250109151203.png]]

nmap 查看对外暴露端口
![[Pasted image 20250109151628.png]]


netstat 查看端口占用
![[Pasted image 20250109151827.png]]

## 2.7 进程管理
![[Pasted image 20250109152215.png]]
![[Pasted image 20250109152229.png]]
![[Pasted image 20250109152440.png]]

## 2.8 主机状态监控
![[Pasted image 20250109155356.png]]
![[Pasted image 20250109155844.png]]
![[Pasted image 20250109155902.png]]
![[Pasted image 20250109160116.png]]
![[Pasted image 20250109160655.png]]
![[Pasted image 20250109160716.png]]

## 2.9 环境变量
![[Pasted image 20250109162555.png]]![[Pasted image 20250109162603.png]]
![[Pasted image 20250109162719.png]]
![[Pasted image 20250109162916.png]]
![[Pasted image 20250109162935.png]] 
![[Pasted image 20250109163449.png]]

## 2.10 压缩和解压缩
![[Pasted image 20250109163556.png]]
### tar 命令

![[Pasted image 20250109163910.png]]
![[Pasted image 20250109163941.png]]
![[Pasted image 20250109164323.png]]
![[Pasted image 20250109164335.png]]
![[Pasted image 20250109164431.png]]