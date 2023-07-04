# git使用指南

## 一、git删除本地文件同步远程文件

### 1、删除单个文件

```shell
$ git rm a1.txt  # 本地仓库删除文件

$ git commit -m 'remove'  # 提交删除

$ git push origin 分支名 # 远程仓库删除
```

Note:*也可以使用删除多个文件的方法*

### 2、删除多个文件

> - 先在本地仓库中删除你要删除的多个文件
>
>      ![](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/20220326204252.png)
>
> - 添加文件`$ git add --all `(提交所有变化)
>
> - 提交文件`$ git commit -m 'a'`（会提示你删除了什么文件）
>
> - `$ git push origin 分支名 `  （这时候远程也删除了这些文件啦）



## 二(1) git删除远程仓库文件同步本地仓库文件

>  1.在远程仓库中删除文件
>
> 2.同步本地仓库文件：拉取远程分支就可以了
>
> ```shell
> $ git pull origin 分支名
> ```

Note:通过网页在远程仓库增删，同步到本地都是拉取分支。

## 二(2) 通过指令删除远程仓库的文件夹

```shell
git rm -r --cached foldername  #如果是删除文件而非文件夹就不用-r
git commit -m "Removed folder from repository"
git push origin master
```

这将从git仓库中删除文件夹并提交更改。





## 三、git revert

**在当前版本下删除git log中的某一条commit的变更操作**

[参考博文](https://blog.csdn.net/yxlshk/article/details/79944535)

****

```shell
#step1:确定反做版本
git revert -n 需要反做的版本号version_name
#step2:修改并且提交反做记录(这一步做完之后,revert的那一条记录的文件就全部不见了)
Revert "version_name对应的commit"
#step3：上推远程仓库
git push origin your-branch
#这样上推的结果就是对应的commit的变动痕迹消失
```

![image-20220328205710138](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220328205710138.png)

## 四、变更git日志(以及作者信息)

> 1. ```shell
>    git rebase -i HEAD~Num  #Num:前面几个版本就输入数字几
>    ```
>
> 2. 其中每一行就是某次提交，把`pick`修改为`edit`，保存退出该文本编辑器。
>
> 3. ```shell
>    git commit --amend  #进入修改
>    ```
>
> 4. ```shell
>    git rebase --continue  #提交
>    ```
>
> 5. 有时候基变会失败，需要git add --all才能解决,一般git会给出提示。
>
> 6. ```shell
>    git log  #检查
>    ```
>
> > 如果需要修改作者的信息
> >
> > >1.把作者信息修改成预期,
> > >` git config user.name 'your new name'  ` ; 
> > >` git config user.email your_email@email `   
> > >2.重新提交日志，`git commit --amend --reset-author `
>
> Note：查看当前的git作者信息
>
> > `git config --list`



## 五、git删除未跟踪的文件

```shell
git clean -nfxd
# -n  加上 -n 参数来先看看会删掉哪些文件，防止重要文件被误删
# -f 删除 untracked files
# -fd 删除 untracked files
# -xfd 连 gitignore 的untrack 文件/目录也一起删掉 （慎用，一般这个是用来删掉编译出来的 .o之类的文件用的）
```



## 六、删除分支

```shell
# 删除一个本地分支
git branch -d 本地分支名

# 删除不掉强制删除本地一个分支
git branch -D 本地分支名

# 删除远程分支
git push origin --delete 远程分支名
```



## 七、撤销git add操作

```shell
git reset HEAD
```

