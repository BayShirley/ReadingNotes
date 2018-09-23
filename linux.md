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
    
    

