# Linux开发指令

## 1、MTK7682/ESP32 prj

- 华为产测服务器：ws://139.9.69.238:8089

	命令行接入指令 `wscat -c ws://139.9.69.238:8089 ` 
	
	测试站点：http://139.9.69.238:8088/testlocal.htm
	
- 写mac地址

  ```C
  mac_rw A40DBC0DF14D
  ```

  ```C
  mac_rw A40DBC0D8562
  ```

  

- 心跳包

  ```C
  MAC &*#S#*&{"command":5,"airGuid":"A4:0D:BC:0D:85:62"}&*#E#*&
  ```

  ```C
  MAC &*#S#*&{"command":5,"airGuid":"A4:0D:BC:0D:F1:4D"}&*#E#*&
  ```

  

- 获取设备ID

  ```C
  PULL &*#S#*&{"command":7,"airGuid":"A4:0D:BC:0D:F1:4D","pullData":{"airGuid":"A4:0D:BC:0D:F1:4D","userID":0,"option":1}}&*#E#*&
  ```

  ```C
  PULL &*#S#*&{"command":7,"airGuid":"A4:0D:BC:0D:85:62","pullData":{"airGuid":"A4:0D:BC:0D:85:62","userID":0,"option":1}}&*#E#*&
  ```

  

- 获取升级信息

	```C
	PULL &*#S#*&{"command":6,"airGuid":"A4:0D:BC:0D:85:62","pullData":{"fwModule":"kew","bwVersion":"HW-V7.5"}}&*#E#*&
	```
	
	```C
	PULL &*#S#*&{"command":6,"airGuid":"A4:0D:BC:0D:F1:4D","pullData":{"fwModule":"kew","bwVersion":"HW-V7.5"}}&*#E#*&
	```


## 2、 Sigmastar LCD prj

### (1)编译工程

```shell
/project$   make image  # 连同kernel一起编译
/project$   make image-fast # 不编译kernel，只编译工程文件
/project$   ./make_usb_factory_sigmastar.sh # 打包烧录文件
```

### (2) 烧录

每次烧录都需要进入uboot模式执行指令

```shell
 setenv ota_upgrade_status 1
 saveenv
 reset
```

### （3）Kernel menuconfig

- step1:进入kernel/目录,执行 build_elsa_lcd.sh脚本中编译kernel部分

	```shell
	declare -x ARCH="arm"
	declare -x CROSS_COMPILE="arm-linux-gnueabihf-"
	make pioneer3_ssc020a_s01a_spinand_demo_camera_defconfig
	```

- step2:

	```shell
	make menuconfig
	```

- step3:修改menuconfig中的配置项，保存后退出
- step4:对比kernel目录下的.config文件把修改项配置到kernel\arch\arm\configs\pioneer3_ssc020a_s01a_spinand_demo_camera_defconfig中去

 ![image-20220521110919174](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220521110919174.png)

- step5:在/project目录下执行编译指令

	```shell
	make image -j15
	```

	

## 3、服务器samba服务重启     

```shell
cd /etc/init.d
./smbd restart
```

## 4、python版本软连接切换

```
sudo update-alternatives --config python
```

输入需要切换的python版本的序号。

## 5、Linux环境变量设置

```shell
setenv gatewayip （ip）   
setenv ipaddr （ip）  
setenv netmask （ip）  
setenv serverip（ip）  
saveenv 
```

## 6、mnt挂载

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

-  如果设备没有自动获取ip，执行：

  ```shell
  udhcpc -i eth0(或者是wlan0等网卡名称)
  ```

  

## 7、设备开关网卡

> Ifconfig 网络接口名 up 命令用于启动网络接口等同于ifup
>
>  Ifconfig 网络接口名 down 命令用于停用网络接口等同于ifdown
>
>    `ifconfig eth0 up/down`     
>
>  `ifup/ifdown eth0`



## 8、文件夹解压缩

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
>    >   a：
>    >     表示add命令，即新建一个压缩文件，该压缩文件存放在当前目录下
>    >   -r：
>    >     表示遍历所有的子目录，每个文件都执行压缩操作，添加到压缩文件中。
>    >   -mx：
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
>
> 6. xz
>
>    > - 创建tar.xz文件
>    >   - 执行`tar cvf xxx.tar xxx`，创建出`xxx.tar文件`，然后使用`xz -z xxx.tar xxx`把`xxx.tar`文件压缩成`xxx.tar.xz`
>    > - 解压tar.xz文件
>    >   - 执行`xz -d xxx.tar.xz`将`xxx.tar.xz`解压成`xxx.tar`，再执行`tar vxf xxx.tar`解压的到文件夹

## 9. iMx6ull pro

### (1)交叉编译工具链

**第三条的路径要随着环境变化**

```shell
export ARCH=arm
export CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
export PATH=$PATH:/home/share/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
```

在~/.bashrc末行插入环境变量

接着执行`source ~/.bashrc`

最后验证工具链是不是配置完成了

```shell
arm-buildroot-linux-gnueabihf-gcc -v
```

显示如下则表示配置完成了

![image-20220605203712106](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220605203712106.png)
