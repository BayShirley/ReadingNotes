# git

@(示例笔记本)[马克飞象|帮助|Markdown]

**马克飞象**是一款专为印象笔记（Evernote）打造的Markdown编辑器，通过精心的设计与技术实现，配合印象笔记强大的存储和同步功能，带来前所未有的书写体验。特点概述：
 
- **功能丰富** ：支持高亮代码块、*LaTeX* 公式、流程图，本地图片以及附件上传，甚至截图粘贴，工作学习好帮手；
- **得心应手** ：简洁高效的编辑器，提供[桌面客户端][1]以及[离线Chrome App][2]，支持移动端 Web；
- **深度整合** ：支持选择笔记本和添加标签，支持从印象笔记跳转编辑，轻松管理。

-------------------

[TOC]

## 初次运行
### 设置用户信息
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
### 查看用户信息
git config --list
## 获取帮助
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
例如，要想获得 config 命令的手册，执行
$ git help config
## 快速命令
### 初始化仓库
对现有的项目进行管理，你只需要进入该项目目录并输入
$ git init 
### 提交暂存区
$ git add [文件名]
### 提交仓库
$ git commit -m
$ git commit -a -m 跳过暂存
### 克隆
$ git clone [url]
### 查看状态
$ git status
#### 查看尚未暂存的文件更新了哪些部分
$ git diff
也就是修改之后还没有暂存起来的变化内容。
#### 查看已暂存的将要添加到下次提交里的内容
$ git diff --cached/staged
#### 状态简览
$ git status -s 
## 从暂存区移除并且不在追踪
$ git rm [文件名] 
$ git rm -f [文件名]
git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：
$ git rm log/\*.log
$ git rm \*~该命令为删除以 ~ 结尾的所有文件
## 撤销操作
### 撤销提交
$ git commit --amend
### 取消暂存的文件
$ git reset HEAD [文件名]
### 撤消对文件的修改/检出
$ git checkout -- [文件名]
## 远程操作
### 远程仓库URL
$ git remote -v
### 添加远程仓库
$ git remote add [远程仓库本地名字,该名字可以用于远程操作的代称] [远程仓库url] 
### 远程抓取
$ git fetch [远程仓库本地名字]
不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作
### 远程推送
$ git push [远程仓库本地名字] [要推送的本地分支]
###  查看远程仓库
$ git remote show [远程仓库本地名字]

```java
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master                         (up to date)
```
它告诉你正处于 master 分支，并且如果运行 git pull，就会抓取所有的远程引用，然后将远程 master 分支合并到本地 master 分支。 它也会列出拉取到的所有远程引用
### 远程仓库移除
$ git remote rm [远程仓库本地名]
### 远程仓库重命名
$ git remote [远程仓库本地名字] [新的远程仓库本地名字]
## 打标签
### 列出标签
$ git tag
TODO
## 分支
### 创建分支
$ git branch [新的分支名]
### 切换分支
$ git checkout [要切换到的分支名]
#### 新建一个分支并同时切换到那个分支上
$ git checkout -b iss53
### 合并分支
你只需要检出到你想合并入的分支，然后运行 git merge 命令
``` java
eg:
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

### 查看分支历史
$ git log --oneline --decorate --graph --all
``` java
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```
### 查看每一个分支的最后一次提交
``` java
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

TODO
## 重命名文件
$ git mv [文件名] [新文件名]
## 查看提交历史
$ git log
$ git log -p -2  -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交
$ git log --stat 次提交的简略的统计信息
$ git log --pretty=oneline指定使用不同于默认格式的方式展示提交历史,oneline 将每个提交放在一行显示另外还有 short，full 和 fuller 可以用 
$git log --pretty=format:"%h - %an, %ar : %s"format，可以定制要显示的记录格式
当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。 这个选项添加了一些ASCII字符串来形象地展示你的分支、合并历史
``` java
eg:
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
eg:
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
``` 
## 忽略文件
可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。文件内容如下举例：
``` java
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```
所有空行或者以 ＃ 开头的行都会被 Git 忽略。

可以使用标准的 glob 模式匹配。

匹配模式可以以（/）开头防止递归。

匹配模式可以以（/）结尾指定目录。

要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。
(所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（*) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z, a/b/z 或 `a/b/c/z`等。GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表，你可以在 https://github.com/github/gitignore)