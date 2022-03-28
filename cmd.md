## Linux开发指令

0. ### 服务器samba服务重启     
```shell
cd /etc/init.d
./smbd restart
```


1. ### Linux环境变量设置

   ```shell
   setenv gatewayip （ip）   
   setenv ipaddr （ip）  
   setenv netmask （ip）  
   setenv serverip（ip）  
   saveenv 
   ```
   
2. ### mnt挂载
   
   ```shell
   mount -t nfs -o nolock -o tcp 10.136.5.23:/workspace1/siqing/nfs /mnt/
   ```
   
   挂载虚拟机
   
   ```shell
   mount -t nfs -o nolock -o tcp 192.168.123.135:/home/string/nfs /mnt/
   ```

   虚拟机接入小米路由器
      ```shell
   mount -t nfs -o nolock -o tcp 192.168.31.228:/home/string/nfs /mnt/
   ```

【空中花园写mac地址】
mac_rw a40dbc068562
   
3. ### 变更git日志

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
   >> 如果需要修改作者的信息
   >>>1.把作者信息修改成预期,
   >>>` git config user.name 'your new name'  ` ; 
   >>>` git config user.email your_email@email `   
   >>>2.重新提交日志，`git commit --amend --reset-author `

4. ### 设备开关网卡

   > Ifconfig 网络接口名 up 命令用于启动网络接口等同于ifup
   >
   >  Ifconfig 网络接口名 down 命令用于停用网络接口等同于ifdown
   >
   >    `ifconfig eth0 up/down`     
   >
   >  `ifup/ifdown eth0`

   

5. ### 文件夹解压缩

   > 1. #### gzip
   >
   >    > - 压缩后的格式为：*.gz
   >    >
   >    > - 这种压缩方式不能保存原文件；且不能压缩目录
   >    >
   >    > - 命令举例：
   >    >
   >    >   ```shell
   >    >   #压缩
   >    >   [root@localhost tmp]# gzip buodo
   >    >   [root@localhost tmp]# ls
   >    >   buodo.gz
   >    >   #解压
   >    >   [root@localhost tmp]# gunzip buodo.gz 
   >    >   [root@localhost tmp]# ls
   >    >   buodo
   >    >   ```
   >    >
   >    >   
   >
   > 2. #### tar
   >
   >    > - 命令选项：
   >    >
   >    >   ```shell
   >    >       -z(gzip)      用gzip来压缩/解压缩文件
   >    >       -j(bzip2)     用bzip2来压缩/解压缩文件
   >    >       -v(verbose)   详细报告tar处理的文件信息
   >    >       -c(create)    创建新的档案文件
   >    >       -x(extract)   解压缩文件或目录
   >    >       -f(file)      使用档案文件或设备，这个选项通常是必选的。
   >    >   ```
   >    >
   >    >   - 命令举例：
   >    >
   >    >   ```shell
   >    >   #压缩
   >    >   [root@localhost tmp]# tar -zvcf buodo.tar.gz buodo
   >    >   [root@localhost tmp]# tar -jvcf buodo.tar.bz2 buodo 
   >    >   #解压
   >    >   [root@localhost tmp]# tar -zvxf buodo.tar.gz 
   >    >   [root@localhost tmp]# tar -jvxf buodo.tar.bz2
   >    >   ```
   >
   > 3. #### zip
   >
   >    > - 与gzip相比：1）可以压缩目录； 2）可以保留原文件；
   >    >
   >    > - 选项：
   >    >
   >    >   ```shell
   >    >   -r(recursive)    递归压缩目录内的所有文件和目录
   >    >   ```
   >    >
   >    > - 命令举例：
   >    >
   >    >   ```shell
   >    >   #压缩和解压文件
   >    >   [root@localhost tmp]# zip boduo.zip boduo
   >    >   [root@localhost tmp]# unzip boduo.zip
   >    >   
   >    >   #压缩和解压目录
   >    >   [root@localhost tmp]# zip -r Demo.zip Demo
   >    >     adding: Demo/ (stored 0%)
   >    >     adding: Demo/Test2/ (stored 0%)
   >    >     adding: Demo/Test1/ (stored 0%)
   >    >     adding: Demo/Test1/test4 (stored 0%)
   >    >     adding: Demo/test3 (stored 0%)
   >    >   [root@localhost tmp]# unzip Demo.zip 
   >    >   Archive:  Demo.zip
   >    >      creating: Demo/
   >    >      creating: Demo/Test2/
   >    >      creating: Demo/Test1/
   >    >    extracting: Demo/Test1/test4        
   >    >    extracting: Demo/test3  
   >    >   ```
   >    >
   >    >   
   >
   > 4. #### bzip2
   >
   >    > - 压缩后的格式：.bz2
   >    >
   >    > - 参数
   >    >
   >    >   ```shell
   >    >   -k    产生压缩文件后保留原文件
   >    >   ```
   >    >
   >    > - 命令举例
   >    >
   >    >   ```shell
   >    >   #压缩
   >    >   [root@localhost tmp]# bzip2 boduo
   >    >   [root@localhost tmp]# bzip2 -k boduo
   >    >   
   >    >   #解压
   >    >   [root@localhost tmp]# bunzip2 boduo.bz2 
   >    >   ```
   >
   > 5. #### 7z
   >
   >    > - 参数：
   >    >
   >    >   ```shell
   >    >   - a：
   >    >     表示add命令，即新建一个压缩文件，该压缩文件存放在当前目录下
   >    >   - -r：
   >    >     表示遍历所有的子目录，每个文件都执行压缩操作，添加到压缩文件中。
   >    >   - -mx：
   >    >     表示压缩等级，9级是最高等级。默认等级是5。
   >    >   ```
   >    >
   >    > - 命令举例
   >    >
   >    >   ```shell
   >    >   #压缩
   >    >   [root@localhost tmp]# 7z a package.7z .\product\* -r -mx=9    
   >    >   # 将当前product文件夹下所有文件压缩到package.7z，package.7z中的文件名不包含product\前缀。
   >    >   
   >    >   #解压
   >    >   [root@localhost tmp]# 7z a package.7z .\product\   
   >    >   #将当前product文件夹下所有文件压缩到package.7z，package.7z中的文件名包含product\前缀。
   >    >   ```