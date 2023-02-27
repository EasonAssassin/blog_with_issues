# [初次使用issues写博客](https://github.com/EasonAssassin/blog_with_issues/issues/1)

# [初次使用issues写博客](https://github.com/EasonAssassin/blog_with_issues/issues/1)

## 目标

- 根据issues标签自动编排README.md中的blog链接
- 自动将isuues生成的文档放到项目BACKUP目录下
- 我当前的blog链接编排布局如下：
   - Gitblog：首行
   - 最近更新：根据创建日期选取最近的5篇blog
   - 其他标签：其他自定义标签所对应的blog
   - TODO：根据TODO标签所展示的blog

## 步骤参考

> https://www.jianshu.com/p/8f1079b7efe1

## 步骤修复

> 完全按照上述文章的步骤操作，发现如下问题，需要以下步骤进行修复：


- 需要新建项目根目录下的空文件README.md

- 配置后发现README.md未更新，查看Actions发现是push denied：原因在于github actions对项目没有写入权限。

  - 需要到项目的settings -> Actions -> General -> Workflow permissions -> 勾选read and write permissions

![image](https://user-images.githubusercontent.com/26082007/221498350-f61cbfdb-e919-422e-95e9-6bfb4016a3cd.png)

- 首页README.md的博客链接编排请参考main.py中的main方法

## 最佳实践

- 通过本地的markdown编辑器编写blog
- 复制blog中的内容，并创建对应的issue
- 配置blog对应的标签（如有）