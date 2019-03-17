# git常规操作

## tag
```shell
git tag -a v0.0.1 -m “init lib”
git tag # 查看当前分支下所有tag
git show v0.0.1 # 查看 v0.0.1 的tag
git tag -d v0.0.1 # 删除tag
git tag -a v0.0.2 9fbc3d0 # 给指定的commit 打tag
git push origin v0.0.2 # 将v0.0.2 标签提交到git服务器
git push origin -tags # 将本地所有标签一次性提交到git服务器
git checkout tagname # 切换到某个tag标签对应的代码
```

## 查看git log

```shell
git log —graph -p -4 --author=tt --before=2018-01-01
```

## 去除 git pull 形成的合并分支

```shell
git pull -r
```

## git pull -r 冲突解决

```shell
git add .
git rebase --continue
# 回到原先分支，然后 
git push
```
## ssh keygen

创建 key : 
```shell
ssh-keygen -t rsa -C "your_email@youremail.com" 
# 密码建议直接回车
```

- 问题一：

```
Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights
```
解决一：
```shell
ssh-add ~/.ssh/id_rsa_gitout   # 秘钥文件
```

