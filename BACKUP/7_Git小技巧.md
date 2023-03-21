# [Git小技巧](https://github.com/EasonAssassin/blog_with_issues/issues/7)

> 小技巧系列：使用git时常用的技巧

## 1. Git clone --depth 1加速后遗症完美解决

> 参考公众号【神光的编程秘籍】；

- `git clone`超大项目时，clone会很慢，此时通过`git clone --depth 1`来加速，效果明显；
- 但同时引入了一些问题：
  - 切换不到历史commit
  - 切换不到别的分支
- 原因在于：
  - git是通过commit存储commit，tree存储目录，blob存储文件内容
  - 每个commit关联这些对象的入口
  - `depth 1`只会下载最后一个commit关联的对象，下载内容更少，所以速度快
- 没有历史commit，可以通过`git pull --unshallow`来解决
- 切换不到其他分支是因为fetch配置导致的，修改`.git/config`中的`fetch = +refs/heads/*:refs/remotes/origin/*`，或者使用命令行`git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*`
- 此时通过`git fetch`即可获取远程所有分支

**修复建议**
- clone超大代码库后，后续push代码，`depth 2`会比`depth 1`快很多。因为`depth 1`下载的commits的第一个是被修改过的"家根节点"，push的时候跟服务器diff对不上导致需要全量发送整个repo文件。但是`depth 2`得第二个commit跟服务器能对上，push时就可以顺利找出diff发送增量部分的文件。

**完善方法**
- `git clone --filter=blob:none`可以只下载commits历史以及HEAD的文件，但不下载其他源码文件。后期可以查看commits历史以及切换分支，但是切换分支时会触发一次从remote下载源文件

## 2. Git log进阶

- `git log`：可以通过--author、--before、--after、--grep、--merges、--no-merges、--all来过滤某个作者、某个时间段、某个commit内容、非merge的commit、全部分支的commit等
  - `--author`：过滤某个作者
  - `--before`、`--after`：过滤某个时间段
  - `--grep`：正则匹配commit内容
  - `--merges`：merge的commit
  - `--no-merges`：非merge的commit
  - `--all`：全部分支的所有commit
  - `--format`：指定输出的颜色和格式
    - `git log --graph --format='%Cred%h%Creset -%C(yellow)%d%Creset %s%Cgreen(%cr) %C(bold blue)<%an>%Creset`
    - 详细参数：
      - %H: commit hash
      - %h: 缩短的commit hash
      - %T: tree hash
      - %t: 缩短的 tree hash
      - %P: parent hashes
      - %p: 缩短的 parent hashes
      - %an: 作者名字
      - %aN: mailmap的作者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
      - %ae: 作者邮箱
      - %aE: 作者邮箱 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
      - %ad: ⽇期 (–date= 制定的格式)
      - %aD: ⽇期, RFC2822格式
      - %ar: ⽇期, 相对格式(1 day ago)
      - %at: ⽇期, UNIX timestamp
      - %ai: ⽇期, ISO 8601 格式
      - %cn: 提交者名字
      - %cN: 提交者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
      - %ce: 提交者 email
      - %cE: 提交者 email (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
      - %cd: 提交⽇期 (–date= 制定的格式)
      - %cD: 提交⽇期, RFC2822格式
      - %cr: 提交⽇期, 相对格式(1 day ago)
      - %ct: 提交⽇期, UNIX timestamp
      - %ci: 提交⽇期, ISO 8601 格式
      - %d: ref名称
      - %e: encoding
      - %s: commit信息标题
      - %f: sanitized subject line, suitable for a filename
      - %b: commit信息内容
      - %N: commit notes
      - %gD: reflog selector, e.g., refs/stash@{1}
      - %gd: shortened reflog selector, e.g., stash@{1}
      - %gs: reflog subject
      - %Cred: 切换到红⾊
      - %Cgreen: 切换到绿⾊
      - %Cblue: 切换到蓝⾊
      - %Creset: 重设颜⾊
      - %C(…): 制定颜⾊, as described in color.branch.* config option
      - %m: left, right or boundary mark
      - %n: 换⾏
      - %%: a raw %
      - %x00: print a byte from a hex code
      - %w([[,[,]]]): switch line wrapping, like the -w option of git-shortlog(1)

- `git shortlog`：是`git log`的统计结果，可以按照作者来分组统计。比如上一周某个人提交了多少commit：
  - `git shortlog --all -n --after="2023-03-01" --before="2023-03-31"  --no-merges --format="%h %as %s"`
  - `git shortlog --all -n --since="30 days ago" --no-merges --format="%h %as %s"`

- `git reflog`：记录ref的变化历史，比如分支切换、reset、新的commit等都会记录下来，也可以直接在`.git/logs/refs`下查看
  - 常用场景：比如通过`git reset`到了一个之前的commit，又想恢复回去，这时候不知道最开始的那个commit hash是啥了，这时候就可以看reflog里的ref变化历史，找到之前的commit hash。
