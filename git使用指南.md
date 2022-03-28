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
>     ![](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/20220326204252.png)
>
> - 添加文件`$ git add --all `(提交所有变化)
>
> - 提交文件`$ git commit -m 'a'`（会提示你删除了什么文件）
>
> - `$ git push origin 分支名 `  （这时候远程也删除了这些文件啦）



## 二、git删除远程仓库文件同步本地仓库文件

>  1.在远程仓库中删除文件
>
> 2.同步本地仓库文件：拉取远程分支就可以了
>
> ```shell
> $ git pull origin 分支名
> ```

Note:通过网页在远程仓库增删，同步到本地都是拉取分支。

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