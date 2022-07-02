# iMx6ull开发指令

## NFS挂载

```shell
mount -t nfs -o nolock,vers=3 192.168.123.39:/home/book/nfs_rootfs /mnt
```

## 配置交叉编译工具链

### 永久生效

```shell
vim  ~/.bashrc
#在末尾添加如下指令
export ARCH=arm
export CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
#添加完之后执行
source  ~/.bashrc
```

### 临时生效

```shell
export ARCH=arm
export CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
```

### 手动指定

```shell
export PATH=$PATH:/home/book/100ask_imx6ull-sdk/ToolChain/arm-buildroot-linux-gnueabihf_sdk-buildroot/bin
make  ARCH=arm CROSS_COMPILE=arm-buildroot-linux-gnueabihf-
```

### 测试交叉编译工具链

#### 1.测试环境变量

```shell
echo $ARCH
echo $CROSS_COMPILE
```

#### 2.测试交叉编译器

```shell
arm-buildroot-linux-gnueabihf-gcc -v
```



