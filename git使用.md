```
安装之后的配置
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

```
$ mkdir learngit
$ cd learngit
$ pwd
$ git init
ls -ah

$ git add readme.txt
$ git commit -m "wrote a readme file"

$ git status
$ git diff readme.txt 
git log [--pretty=oneline]
HEAD表示当前版本
git reflog
 git reset --hard HEAD^ 回退到上个版本
 git reset --hard 1094a 回退到某个版本
```