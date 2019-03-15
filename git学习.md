## git学习
#### 一、git配置

```shell
[user]
    name = xyzoer
    email = ttghost@126.com
[color]
    ui = true
    log = true
    interactiove = true
    branch = true
    status = true
    diff = true
[core]
    excludesfile = /Users/fish/.gitignore_global
[difftool "sourcetree"]
    cmd = opendiff \"$LOCAL\" \"$REMOTE\"
    path =
[mergetool "sourcetree"]
    cmd = /Applications/SourceTree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
    trustExitCode = true
[push]
    default = matching
[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    pl = pull
    ps = push
    dt = difftool
    l = log --stat
    cp = cherry-pick
    ca = commit -a
```
#### 二、常见问题
- 客户端提交，git服务器不同步问题
打开远程git服务器，进入到相应git仓库执行 <code>git co HEAD .</code>，将客户端提交过来数据同步到工作区。    
时时添加添加更新服务器工作区可以如下解决，在git服务器的.git/hooks目录添加<code>post-receive</code>，内容如下：

```shell
#!/bin/sh
# http://debuggable.com/posts/git-tip-auto-update-working-tree-via-post-receive-hook:49551efe-6414-4e86-aec6-544f4834cda3
cd ..
env -i git reset --hard
```
- 远程仓库拒绝提交问题

```shell 
GIT PUSH REMOTE ERROR
```
打开远程仓库<code>.git/config</code>文件添加：

```shell
[receive]
	denyCurrentBranch = warn
```
这样就可以通过git push提交自己的稳定更新，要想在push后在remote端看到更新的效果，执行git reset –hard即可。    
【参考文档】：    
[搭建Git本地服务器](http://www.cnblogs.com/trying/archive/2012/06/28/2863758.html)    
- 多账号冲突解决    
1 Open "Keychain Access.app" (You can find it in Spotlight or LaunchPad)    
2 Select "All items" in Category    
3 Search "git"    
4 Delete every old & strange items    
5 Try to Push again and it just WORKED    
【参考文档】    
[Git's famous “ERROR: Permission to .git denied to user”](http://stackoverflow.com/questions/5335197/gits-famous-error-permission-to-git-denied-to-user)    
