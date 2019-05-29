---
layout: post
title: "Git应用：统一修正提交历史中的用户名和邮箱"
categories: 工具
tags: Git Git应用
excerpt: "介绍利用shell脚本批量重写Git提交历史中的用户名和邮箱的方法。"
author: "Eric Zong"
---

* content
{:toc}

# 说明

当查看你的 Git 库提交历史时，可能会存在这样的问题：不同的提交历史中，用户名和邮箱不是一样的。

如果我们想把所有提交历史中的用户名和邮箱统一，可能吗？

答案是肯定的，Git 很强大，它支持重写历史。

# 脚本

```shell
git clone --bare https://github.com/user/repo.git
cd repo.git
```

上面的命令克隆一个“裸” Git 仓库（注意文件夹带有 .git 后缀），所有操作都将在其上进行。

在 repo.git 根目录创建一个 .sh 脚本文件，并将以下代码拷贝到其中，保存并执行（注意，它是 shell 脚本，需要在 bash 下执行）。

```shell
#!/bin/sh

git filter-branch --env-filter '
declare -a OLD_EMAILS
OLD_EMAILS=('babailong@126.com' 'Eric@Eric-PC' 'Eric@192.168.3.9')
CORRECT_NAME="Eric Zong"
CORRECT_EMAIL="ericzonglu@126.com"
for item in ${OLD_EMAILS[@]};do
    if [ "$GIT_COMMITTER_EMAIL" = "$item" ]
    then
        export GIT_COMMITTER_NAME="$CORRECT_NAME"
        export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$item" ]
    then
        export GIT_AUTHOR_NAME="$CORRECT_NAME"
        export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_COMMITTER_EMAIL" = "$CORRECT_EMAIL" ]
    then
        export GIT_COMMITTER_NAME="$CORRECT_NAME"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$CORRECT_EMAIL" ]
    then
        export GIT_AUTHOR_NAME="$CORRECT_NAME"
    fi
done
' --tag-name-filter cat -- --branches --tags

git push --force --tags origin 'refs/heads/*'
```

说明：

上面的脚本可以批量替换 Git 提交历史中的用户名和邮箱。

OLD_EMAILS 数组指出了提交历史中需要替换的邮箱。

CORRECT_NAME 是替换为的用户名。

CORRECT_EMAIL 是替换为的邮箱。

注意：如果原始提交历史中的邮箱即为需替换为的邮箱，则依然会尝试修正其用户名。

# 应用场景

需要使用以上脚本可能是因为历史提交时未配置用户名和邮箱，或者错误使用了全局配置的用户名和邮箱，或者是邮箱变更，或是用户名不“一致规范”等等。

总之，一切你想将历史提交中的用户名和邮箱统一的时候都可以使用以上脚本。

**需要注意的是，脚本的一个副作用是修改所有提交的校验和，因此，之前克隆的本地库在 pull 时会提示“历史不匹配”而失败。一个简单的处理方式是，脚本执行前提交所有变更，执行脚本后重新克隆。**

# 参考

[《Git Pro (2nd Edition)》 7.6 Git 工具 - 重写历史](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E5%86%99%E5%8E%86%E5%8F%B2#_%E5%85%A8%E5%B1%80%E4%BF%AE%E6%94%B9%E9%82%AE%E7%AE%B1%E5%9C%B0%E5%9D%80)

[CSDN - 修改 github 已提交用户名和邮箱](https://blog.csdn.net/weixin_42470791/article/details/83050729)

[脚本之家 - Bash中数组的操作教程](https://www.jb51.net/article/101241.htm)
