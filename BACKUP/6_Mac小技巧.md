# [Mac小技巧](https://github.com/EasonAssassin/blog_with_issues/issues/6)

> 小技巧系列：我个人使用mac时常用的软件和技巧

## 1. OmniGraffle绘图软件

> 用来绘制图表，流程图，组织结构图以及插图，也可以用来组织头脑中思考的信息，组织头脑风暴的结果，绘制心智图，作为样式管理器，或设计网页或PDF文档的原型。
它具有采用拖放的所见即所得界面。

- 下载：https://xclient.info/s/omnigraffle.html

## 2. Privoxy做中转代理

> 部分代理基本都使用SOCKS5，某些场景下我们需要HTTP代理，如git仅支持HTTP代理。
> 使用Privoxy可以把SOCKS代理，转成HTTP代理。这样就可以提供给git和ios设备使用。

- 下载：`brew install privoxy`
- 配置：修改`/usr/local/etc/privoxy/config`

```shell
# privoxy服务端口。使用0.0.0.0即可在局域网内使用此代理，如只想本机使用，使用127.0.0.1。
listen-address 0.0.0.0:8118
# socks5端口
forward-socks5 / 127.0.0.1:1080 .
```

- 启动：`brew services restart privoxy`

## 3. Proxy快捷键

- 编辑`~/.zshrc`

```shell
alias proxy='export all_proxy=http://127.0.0.1:8118'
alias unproxy='unset all_proxy'
```

## 4. Git快捷键

- 编辑`~/.gitconfig`

```shell
[alias]
        co = checkout
        ci = commit
        st = status
        pl = pull
        ps = push
        dt = difftool
        l = log --stat
        cp = cherry-pick
        ca = commit -a
        b = branch
        ri = rebase -i
        ss = stash
        sp = stash pop
        sl = stash list
        unstage = reset HEAD --
        cache = diff --cached
        tmp = commit -a -m"tmp"
        ff = pull --ff-only
        prev = reset HEAD^
        pr = remote prune origin   
[log]
        date = iso8601
[core]
        editor = vim
[user]
        email = abc@***.com
        name = abc
```

- 使用：`git st #即git status`

## 5. Git自动提交

- 编辑`/usr/local/bin/gta`

```shell
#!/bin/bash

COMMIT=$(if [ "${1}" ]; then echo ${1}; else echo "auto commit"; fi)
BRANCH=$(git symbolic-ref --short -q HEAD)
git add -A
git status

read -r -p "是否继续提交? [Y/n] 自动提交且合并到master分支请按'[a/A]':" input

case $input in
    [nN][oO]|[nN])
        echo "中断提交"
        exit 0
            ;;
    [aA])
        echo "继续提交并自动合并到master分支"
        echo "commit msg: ${COMMIT}"
        echo "current branch: ${BRANCH}"
        git commit -m "${COMMIT}"
        git push origin "${BRANCH}"
        git checkout master
        echo "git checkout master"
        git merge ${BRANCH}
        echo "git merge dev"
        git push origin master
        echo "git push origin master"
        git checkout ${BRANCH}
        echo "git checkout dev"
                    exit 0
        ;;
    [yY][eE][sS]|[yY]|*)
        echo "继续提交"
        echo "commit msg: ${COMMIT}"
        echo "current branch: ${BRANCH}"
        git commit -m "${COMMIT}"
        git push origin "${BRANCH}"
                    exit 0
        ;;
    *)
    echo "输入错误，请重新输入"
    ;;

esac
```

- 使用：`gta "this is test" #即git commit -m "this is test" && git push origin ***`

## 6. Gvm管理go版本

> gvm的本质是shell脚本。其安装go的原理是通过下载github上go的源码，通过git标签的方式来检测go的版本，然后通过源码编译的方式来安装go。
> 所以跟github源码地址的网络连通性，将是决定gvm安装是否顺利的决定性因素。

- 安装：`brew install gvm`
- 加速：编辑`vim ~/.gvm/scripts/install`

```shell
GO_SOURCE_URL=https://github.com/golang/go
GO_BINARY_BASE_URL=${GO_BINARY_BASE_URL:-"https://golang.google.cn/dl/"}
```

- 查看全部版本：`gvm listall`
- 下载指定版本：`gvm install go1.20.2`
- 使用指定版本：`gvm use go1.20.2 --default`

## 7. Bartender

> 用于整理mac右上角图表的工具

- 下载：https://xclient.info/s/bartender.html

![image](https://user-images.githubusercontent.com/26082007/223954618-c9d4274d-c21e-4b35-82a4-5243d29c3e8c.png)


## 8. Lx-music-desktop

> 用于免费听所有完整音乐的神器

- 下载：https://github.com/lyswhut/lx-music-desktop/releases

## 9. QuickTimePlayer和iMovie剪辑

> 我主要用来做录屏教程分享

- 下载：官方App Store下载
- 使用`shift + command + 5`调用QuickTimePlayer进行录屏，得到`.mov`的视频
- `.mov`视频仅能在mac上面识别
- 通过iMovie进行剪辑
- 导出转化成`.mp4`文件：`文件 -> 共享 -> 文件...`

## 10. 翻页时钟

> 锁屏界面显示时钟

- 下载：App Store

## 11. 日历Fantastical 

> 功能更强大的日历软件

- 下载：https://xclient.info/s/fantastical.html

![image](https://user-images.githubusercontent.com/26082007/223973586-444bb72f-c725-4f5c-b91b-4fae13a05c14.png)

## 12. iStatistica

> 显示mac网络、内存、CPU等信息

- 下载：https://xclient.info/s/istatistica-pro.html

![image](https://user-images.githubusercontent.com/26082007/223982037-22d351a6-d26e-476c-9513-85a590fba34e.png)
