# 关联github
```
ssh-keygen -t rsa -C "812747475@qq.com"
cd ~
ls -al
cd .ssh
复制公钥id_rsa.pub到github上
```

```shell
git remote add origin git@github.com:lafeier888/note.git #创建远程仓库
git push origin master #提交本地master分支到远程仓库origin
```

```
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
```



```

git add

git commit

git status

git diff

git remote [-v]

git push
```




