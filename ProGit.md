# 下载地址  
https://git-scm.com/book/en/v2  
https://git-scm.com/book/zh/v2


# 几个关键概念  
>__origin__  
>Git给你clone的仓库服务器的默认名字，如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。对应于git命令参数中的 [remote-name]或者<repository>。“origin” 并无特殊含义，远程仓库名字 “origin” 与分支名字 “master” 一样，在Git 中并没有任何特别的含义一样。同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用，“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o booyah，那么你默认的远程分支名字将会是 booyah/master。  

>__master__  
>Git的默认分支名字，并不是一个特殊分支。 它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。对应于git命令参数中的 [branch-name]

# git图解  
http://marklodato.github.io/visual-git-guide/index-zh-cn.html
