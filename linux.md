# 跟老男孩
ctrl+a 到行首

ctrl+e 到行尾

ctrl+u 在行尾部删除正行

ctrl+k 在行首剪切整行

ctrl+r 搜索用过的命令

ctrl+g 推出ctr+r

ctrl+y 粘贴ctrl+u/k 

ctrl+d 推出

linux命令符由PS1控制。临时修改PS1=’[\u@\h \W]\\$ ‘  永久修改：编辑 /etc/bashrc ,对[ "$PS1" = "\\s-\\v\\\$ " ] && PS1="[\u@\h \W]\\$ "这个内容进行修改。执行source/etc/bashrc使修改生效

/查找内容      向下查找（按n顺向下一个，按N反向下一个。）

?查找内容      向上查找（n，N同上）

pwd 
  
    -L logical逻辑路径，去pwd系统环境变量的直，忽略软连接
    
    -P physical，显示物理路径时，如果当前当前目录路径是软连接，则会显示软连接对应的源文件
    
cd 是 bash shell 的内直命令,查看该命令对应的系统帮助需妥使用 help cd 。

cd

  -P
  
  
  如采切换的目标目录是一个软链接,则会直接切换到软链接指向的真正物理目标目录,和 pwd 命令的,p 选项功能类似
  -L
  
  功能与,p 相反,如采切换的目标目录是一个软链接,则直接切换到软链接所在的目录,和 pwd 命令的-L 选项功能类似,该参数不常用
  
  `-`
  
  当只使用“-”选项时,将会从当前 目录切换到系统环境变量“ OLDPWD ”对应值的目录路径, l!f 当前用户上一次所在的目录路径※
  
  ..
  
  当只使用“..”选项时,将会从当前 目录切换到当前目录的上一级目录所在的路径※
  
  ~
  
  当只使用“~”选项时 ,将会从当前目录切换到系统环境变量“HOME ”对应值的目录路径, l!f 当前用户的家 目录所在的路径※
  
## tree 命令

pm -qa tree 查询是否安装

yum -y install tree 安装

-a 显示所有文件,包括隐藏文件(以“.”点开头的文件)

-d 只显示目录※

-f 显示每个文件的全路径

-i 不显示树枝,常与- f 参数配合使用

-L level 边历目录的最大层数, level 为大于 0 的正整数※

-F 在执行文件 、 目录 、 Socket、 符号连接、 管道名称等不同类型文件的结尾,各自加上“*” 、" "“@ ” 、--、“|”号,类似 ls 命令的-F 选项

## mkdir
-p 递归创建目录,递归的意思是父目录及其子目录及子目录的子目录...··※ 即使妥创建的目录事先已存在也不会报错提示目录已存

-m 设直新创建目录的默认目录对应的权限(eg:mkdir - m 333 dir2)

-v 显示创建目录的过程

## touch

命令有两个功能: 一是创建新的空文件 ; 二是改变已有文件的时间戳属性 。


-a  只更改指定文件的最后访问时间

-d STRING 使用字符串 STRING 代表的时间作为模板设直指定文件的时间属性

-m 只更改指定文件的最后修改时间

-r file 将指定文件的时间属性设直为与模版文件 fi l e 的时间属性相同

-t STAMP
使用[[CC]YY]MMDDhhmm [.ss] 格式的时间设直文件的时 间 属性 。 格式的含义从左
到右依次为 : 世纪 、 年 、 月 、 日 、 时 、 分 、 秒

## ls

GNU/Linux 的文件有 3 种类型的时间戳:
Access: 2015-07-30 17:48:20.502156890 +0800 #〈==最后访问文件的时间 。

Modify: 2015-07-30 17:48:45.006106223 +0800 #〈口m 最后修改文件的对闷 。

Change: 2015-07-30 17:48:45.006106223 +0800 #<==最后改变文件状态的时间 。

对应 ls 命令,查看上述时间戳的选项如下:
mtime :最后修改时间( ls -lt) #<==修改文件内容,文件的修改时间( modify time )会改变 

ctime :状态改变时间( ls -le) It <自嚣修改文件内容、移动文件或改变文件属性等,文件的 change 时l闭会改变 。
atime :最后访问时间( ls -lu) #<==查看文件内容时,文件的访问时间( access t 工 me )会改变。
|-l|使用长格式列 出 文件及 目 录信息※|
|-a|显示目录下的所有文件 ,包括以“.”字符开始的隐藏文件 ※|
|-t|根据最后 的 修改时 间 ( mtime )排序,默认是以文件名排序※ |
|-r|依相反次序排序※|
|-F|在条 目 后加上文件类型 的 指示符号 ( *、人=、@、| ,其中的一个 )※|
|-p|只在目 录后面加 上“/”|
|-i|显 示 inode 节点 信息|
|-d|当遇到目 录 时 ,列 出目 录本身而非 目 录内的文件,并且不跟随符号链接※|
|-h|以人 类 可 读 的 信息显示文件或目录大小,如 l KB 、 234MB 、 2GB等※|
|-A|列 出所有 文件,包括隐藏文件,但不包括“.”与 “.. ”这两个目录|
|-S|根据文件大小排序|
|-R|逆归出所有子目录|
|-x|逐行列出项目而不是逐栏列出|
|-X|根据扩展名排序|
|-c|根据状态改变时间( ctime )排序|
|-u|根据最后访问时间( atime )排序|
|--color={ never,always, auto}|不同的文件类型显示不同的颜色参数, never 表示不显示, always 表示总是显示, auto 表示自动显示|
|--full-time|以完整的时间格式输出|
|--time-style= { full-iso,iso , iso, locale}|以不同的时间格式输出, long-iso 效采最好|
|--time={atime,ctime}|按不同的时间属性输出, atime 表示按访问时间, ctime 表示按改变权限属性时间,如采不加此参数则默认为最后修改时间|

  

