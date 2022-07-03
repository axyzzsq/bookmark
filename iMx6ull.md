# iMx6ull开发指令

## 1.NFS挂载

```shell
mount -t nfs -o nolock,vers=3 192.168.123.39:/home/book/nfs_rootfs /mnt
```

## 2.配置交叉编译工具链

### 2.1永久生效

```shell
vim  ~/.bashrc
#在末尾添加如下指令
export ARCH=arm
export CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
#添加完之后执行
source  ~/.bashrc
```

### 2.2临时生效

```shell
export ARCH=arm
export CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
```

### 2.3手动指定

```shell
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
make  ARCH=arm CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
```

### 2.4测试交叉编译工具链

#### 2.4.1.测试环境变量

```shell
echo $ARCH
echo $CROSS_COMPILE
```

#### 2.4.2.测试交叉编译器

```shell
arm-buildroot-linux-gnueabihf-gcc -v
```

## 3.单独编译更新kernel + dtb 内核模块

### 3.1编译内核镜像

```shell
cd Linux-4.9.88
make 100ask_imx6ull_defconfig
make zImage  -j4
make dtbs
cp arch/arm/boot/zImage ~/nfs_rootfs
cp arch/arm/boot/dts/100ask_imx6ull-14x14.dtb  ~/nfs_rootfs  #不要错把.dts拷过去，没用
```

### 3.2编译安装内核模块

#### 3.2.1 编译内核模块

```shell
cd Linux-4.9.88
make ARCH=arm CROSS_COMPILE=arm-buildroot-linux-gnueabihf- modules
```

####  3.2.2 安装内核模块到Ubuntu某个目录下备用

```shell
cd ~/100ask_imx6ull-sdk/Linux-4.9.88/
sudo make  ARCH=arm INSTALL_MOD_PATH=/home/book/nfs_rootfs  modules_install
```

#### 3.2.3安装内核和模块到开发板上

```shell
#首先进行挂载，然后拷贝文件
cp  /mnt/zImage  /boot
cp  /mnt/*.dtb   /boot
cp  /mnt/lib/modules  /lib  -rfd
sync  #同步文件
reboot #重启后log查看文件编译时间
```

## 4.单独编译更新uboot

### 4.1编译u-boot镜像

```shell
cd Uboot-2017.03
make distclean
make  mx6ull_14x14_evk_defconfig
make
#等编译结束之后把uboot镜像文件u-boot-dtb.imx拷贝到开发板上的home目录下
```

### 4.2 烧录uboot

```shell
echo 0 > /sys/block/mmcblk1boot0/force_ro  #失能分区写保护
dd if=u-boot-dtb.imx of=/dev/mmcblk1boot0 bs=512 seek=2
echo 1 > /sys/block/mmcblk1boot0/force_ro #重新使能分区写保护
reboot
```

系统重启后查看log，可以看到u-boot是刚刚编译完成的

![image-20220703183831397](https://pic-1304959529.cos.ap-guangzhou.myqcloud.com/DB/image-20220703183831397.png)



