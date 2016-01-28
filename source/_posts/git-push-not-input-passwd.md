---
title: git push 提示输入用户名和密码
date: 2016-01-29 03:12:28
tags: [git, git push, github]
---
  本人由于维护一些项目，经常需要提交代码到github服务器，当某些仓库项目不属于ssh协议时，就会出现`git push`要求输入用户名和密码的情况，搜了一下给了个解决办法，这里记录一下。<!-- more -->
```sh
xs@xs-MacBookPro:~/test$ git push origin master
Username for 'https://github.com':
```

### Linux下
1.  在用户跟目录下`~/`创建文件`.git-credentials`，并打开编辑，输入内容：
```sh
https://{username}:{password}@github.com
#例如https://X-s:github110@github.com
```

2.  在终端下执行`git config --global credential.helper store`

3.  可以看到`~/.gitconfig`文件，会多了一项：
```
[credential]

    helper = store
```

4.  此时再去`push`则不用输入账户名和密码了
```sh
xs@xs-MacBookPro:~/test$ git push origin master
Everything up-to-date
```
