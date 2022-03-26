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