# Mac小技巧

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

- 安装：`brew install gvm`
- 加速：编辑`vim ~/.gvm/scripts/install`

```shell
GO_SOURCE_URL=https://github.com/golang/go
GO_BINARY_BASE_URL=${GO_BINARY_BASE_URL:-"https://golang.google.cn/dl/"}
```

- 查看全部版本：`gvm listall`
- 下载指定版本：`gvm install go1.20.2`
- 使用指定版本：`gvm use go1.20.2 --default`

<script type="text/javascript" id="clustrmaps" src="//clustrmaps.com/map_v2.js?d=7VuDJtnt0IGkUS1uYneOF7JJ8lXJuzkE93uO9Hyoe0k&cl=ffffff&w=a"></script>
<style>
    <script type="text/javascript" id="clustrmaps" src="//clustrmaps.com/map_v2.js?d=7VuDJtnt0IGkUS1uYneOF7JJ8lXJuzkE93uO9Hyoe0k&cl=ffffff&w=a"></script>
</style>