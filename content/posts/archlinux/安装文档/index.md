---
title: 安装文档
date: 2024-08-04
author: ageha
tags: [ archlinux, 安装文档 ]
summary: 简洁明了的 archlinux 安装文档
cover:
    image: "images/cover.png"
    zoom: 100
---

#### 字体设置
```bash
setfont ter-132n # 暂时设置
```
`/etc/vconsole.conf`的`FONT`变量永久性设置字体
```bash
...
FONT=ter-132n
```
安装以下字体：
``` bash
 pacman -S ttf-jetbrains-mono terminus-font ttf-font-awesome
```
#### 联网
``` bash
iwctl
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <要连接的WiFi>
```
#### 更新系统时间
```bash
timedatectl set-ntp true
```
#### 分区
```bash
lsblk # 查看分区
cfdisk # 创建分区
mkfs.ext4 /dev/根分区
mkfs.fat -F 32 /dev/EFI分区
```
```bash
mount /dev/根分区 /mnt # 挂载根分区
mkdir /mnt/boot # 创建启动分区
mount /dev/EFI分区 /mnt/boot # 再挂载（lsblk查看windows的EFI分区号）
```
#### 换源
``` bash
nano /etc/pacman.d/mirrorlist
pacman -S archlinux-keyring
```
#### 安装
```bash
pacstrap /mnt sudo neovim terminus-font base linux linux-firmware # 基本安装 可选内核
```
```bash
genfstab -U /mnt >> /mnt/etc/fstab # 生成fstab文件
cat /mnt/etc/fstab # 检查一下
```
#### 配置系统
``` bash
arch-chroot /mnt # Change root 到新安装的系统
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime # 设置时区
hwclock --systohc # 生成 /etc/adjtime
```
#### 本地化
编辑 `/etc/locale.gen`，然后取消掉 `en_US.UTF-8 UTF-8` 和其他需要的区域设置前的注释，接着：
```bash
locale-gen
```
然后创建`/etc/locale.conf`文件，并编辑设定 LANG 变量：
```bash
LANG=en_US.UTF-8
```
#### 网络配置
``` bash
pacman -S networkmanager
systemctl enable NetworkManager.service
# nmtui
```
#### 设置 Root 密码
``` bash
passwd
```
#### Grub
> 克隆分区后修复时，先删除 /boot 目录后运行以下命令！

``` bash
pacman -S grub efibootmgr # 安装
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub # 安装到
grub-mkconfig -o /boot/grub/grub.cfg # 生成配置文件
```
#### 重启
``` bash
exit # 退出chroot环境
umount -R /mnt # 卸载被挂载分区
reboot
```
#### 用户管理
``` bash
useradd -m -G wheel ageha
passwd ageha
EDITOR=nvim visudo # 删除含 %wheel 行注释
```


