# SDK 整机编译烧写流程

Created: Mar 24, 2021 11:27 AM
Created By: 鑫 郭
Last Edited By: 鑫 郭
Last Edited Time: Mar 24, 2021 2:37 PM
Status: In Review 👀
Type: Technical Spec

# 编译各分区镜像文件

根目录下使用 [`build.sh`](http://build.sh/) 进行 app.jffs2 rootfs.jffs2 u-boot uImage 分区文件的编译。编译完成后目标文件生成在 `SDK根目录/out` 下

```bash
# 清除生成过程文件
sudo ./build.sh clean
# 编译所有文件
./build.sh
```

# 烧写方式一 整机正常运行下 使用网络传输烧写

机器正常运行时，提供 `telnet` 登录端口

```bash
# telnet 链接同局域网下的台灯
telnet 192.168.243.171

# 输入登录密码
login: admin
Password:admin

# 使用 mount 指令挂载电脑 NFS 文件夹
mount -t nfs -o nolock -o tcp NFS服务器IP:NFS文件夹路径 /mnt

# 进入 home 目录进行文件复制
cd /home/
```

分区文件对应关系如下

```bash
0x000000000000-0x000000040000 : "bootstrap"
0x000000040000-0x000000050000 : "uboot-env"
0x000000050000-0x000000090000 : "uboot" /dev/mtd2
0x000000090000-0x000000390000 : "kernel" /dev/mtd3
0x000000390000-0x000000690000 : "rootfs" /dev/mtd4
0x000000690000-0x000000f90000 : "app" /dev/mtd5
```

## 复制镜像文件进行烧写

> 假设烧写方式一导致的启动失败，使用烧写方式二

### uboot 烧写

复制 `u-boot.img` 文件，使用台灯自带的 `flash_erase flashcp` 软件烧写

> NOTE: 老旧机型可能没有内置 flash 软件，可以在 SDK `yqsf-main/board_support/rootfs/rootfs_pub/sbin/` 路径下找到

```bash
cp /mnt/u-boot.img /home/
# 擦除
flash_erase /dev/mtd2 0x0 4
# 烧写
flashcp -v u-boot.img /dev/mtd2
```

### kernel 烧写

```bash
cp /mnt/out/uImage /home/
# 擦除
flash_erase /dev/mtd3 0x0 30
# 烧写
flashcp -v uImage /dev/mtd3
```

### app 分区烧写

```bash
cp /mnt/out/app.jffs2 /home/
# 擦除
flash_erase /dev/mtd5 0x0 90
# 烧写
flashcp -v app.jffs2 /dev/mtd5
```

### rootfs 烧写

> rootfs 与系统运行软件有关，最后一个烧写

```bash
cp /mnt/out/rootfs.jffs2 /home/
# 擦除
flash_erase /dev/mtd4 0x0 30
# 烧写
flashcp -v rootfs.jffs2 /dev/mtd4
```

# 烧写方式二 uboot 下使用串口传输烧写

![SDK%20%E6%95%B4%E6%9C%BA%E7%BC%96%E8%AF%91%E7%83%A7%E5%86%99%E6%B5%81%E7%A8%8B%200c597bd2d21f472289e779ec24cb4f19/UART.png](SDK%20%E6%95%B4%E6%9C%BA%E7%BC%96%E8%AF%91%E7%83%A7%E5%86%99%E6%B5%81%E7%A8%8B%200c597bd2d21f472289e779ec24cb4f19/UART.png)

链接串口后，开机前长按键盘U输入进入u-boot模式

输入 loady 进入接收文件模式，串口工具使用 ymodem 可以传输文件，每传输完成一个文件后，文件会暂存在a1000000开始的地址 可以使用 u-boot指令工具烧写

> uboot文件 烧写失败后系统启动会自动进入 loady 指令模式，这时需要传输 u-boot.img 文件，如果开机不在这个模式需要检查 flash 是否损坏

```bash
# uboot:
sf probe 0
sf erase 50000 40000
sf write a1000000 50000 40000
# kernel:
sf probe 0
sf erase 90000 300000
sf write a1000000 90000 300000
# rootfs.jffs2
sf probe 0
sf erase 390000 300000
sf write a1000000 390000 300000
# app.jffs2
sf probe 0
sf erase 690000 900000
sf write a1000000 690000 900000

# 重启
reset
```