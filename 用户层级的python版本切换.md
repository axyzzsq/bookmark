# 用户层级的python版本切换

> 在用户目录下以源码安装的方式创建自己不同的python版本，切换不受系统管理控制，也不影响其他用户。

## 一、源码安装python2.7

1. 到[Python官网2.7版本目录](https://www.python.org/downloads/release/python-2718/)下载源码包XZ_compressed_source_tarball，到ubuntu中解压Python-2.7.18.tar.xz得到Python-2.7.18文件夹；

	![image-20220513160923812](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513160923812.png)

2. 在当前用户目录下创建myTools/python2目录

	 ![image-20220513161658642](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513161658642.png)

3. 返回第1步解压得到的文件Python-2.7.18中，指定安装python2.7到自定义的目录

	`./configure --prefix=/home/siqing/myTools/python2 ` 

4. ` make -j12 && make install`

5. `vim ~/.bashrc`,在末行增加环境变量

	`export PATH=/home/siqing/myTools/python2/bin:$PATH`

6. `source ~/.bashrc`

7.  查看版本号

	 ![image-20220513162552926](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513162552926.png)

	

## 二、源码安装python3.8

执行同上 1.  2.  3.  4.步骤，在/home/siqing/myTools/python3中安装python3版本



## 三、python2.7切换到python3.8版本

1. `vim ~/.bashrc`

2. 末行修改指向的安装路径为3.8的安装路径

	![image-20220513165451395](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513165451395.png)

3. `source ~/.bashrc` 

4. 查看python版本，如果还是2.7，重启终端，再次查看

	![image-20220513165932143](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513165932143.png)

	

	![image-20220513170003971](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220513170003971.png)

