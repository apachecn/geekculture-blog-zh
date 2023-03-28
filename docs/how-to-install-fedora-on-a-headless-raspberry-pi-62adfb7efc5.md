# 如何将 Fedora 安装在无头树莓 Pi 上

> 原文：<https://medium.com/geekculture/how-to-install-fedora-on-a-headless-raspberry-pi-62adfb7efc5?source=collection_archive---------4----------------------->

## 在无头的 Raspberry Pi 3 或 4 上安装 Fedora 的完整指南(不需要键盘、显示器或串行电缆)

![](img/4523980e9f1025ee8cd75a25c4546e1a.png)

Vector by [Vecteezy](https://www.vecteezy.com)

# 概观

我找不到一个完全没头没脑的更新版的 Raspberry Pi 的 Fedora 安装指南。我发现的那些不工作或需要串行电缆，这违背了目的。经过大量的研究和一个试错的过程，我成功地在一个无头的 Raspberry Pi 3 和 4 上安装了 Fedora Server 34。

在这个分步指南中，我将向您展示如何下载正确的映像，安装它，修改它以满足我们的需求，并在 microSD 卡上安装自定义映像，为您的 Raspberry Pi 做好准备。作为结尾的奖励，我将分享设置 Wi-Fi 连接的步骤。

# 先决条件

要遵循本指南，您需要:

*   带 microSD 读卡器的 Fedora 机器
*   Fedora ARM 原始图像
*   带有有线互联网连接的 Raspberry Pi 3 或 4

# 步骤 1 —下载 Fedora ARM 映像

对于 ARMv7 (32 位)图像，请访问 [Fedora ARM](https://arm.fedoraproject.org) 网站。如果你像我一样喜欢 64 位图像，它们有点隐藏。相反，访问 [Fedora Wiki](https://fedoraproject.org/wiki/Architectures/ARM/Raspberry_Pi?rd=Raspberry_Pi#aarch64_supported_images_for_Raspberry_Pi_3) 并选择您的类型:工作站、服务器或小型。

在我的环境中，我运行这个命令来下载最新的 Fedora Server 64 位原始图像:

```
curl -L -O [https://download.fedoraproject.org/pub/fedora/linux/releases/34/Server/aarch64/images/Fedora-Server-34-1.2.aarch64.raw.xz](https://download.fedoraproject.org/pub/fedora/linux/releases/34/Server/aarch64/images/Fedora-Server-34-1.2.aarch64.raw.xz)
```

# 步骤 2 —准备原始图像

首先，解压缩图像:

```
xz --decompress Fedora-Server-34-1.2.aarch64.raw.xz
```

上面的命令将 789 MB 的`.xz`文件替换为 7 GB 的`.raw`文件。

接下来，用`fdisk`列出原始映像中的分区:

```
$ **fdisk -l Fedora-Server-34-1.2.aarch64.raw** 
Disk Fedora-Server-34-1.2.aarch64.raw: 7 GiB, 7516192768 bytes, 14680064 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd17792e4Device                            Boot   Start      End  Sectors  Size Id Type
Fedora-Server-34-1.2.aarch64.raw1 *       2048  1230847  1228800  600M  6 FAT16
Fedora-Server-34-1.2.aarch64.raw2      1230848  3327999  2097152    1G 83 Linux
Fedora-Server-34-1.2.aarch64.raw3      3328000 14680063 11352064  5.4G 8e Linux LVM
```

我们的图像有三个分区。`.raw1`是引导程序分区。`.raw2`是 Linux 引导分区(id 83)。最后，`.raw3`是我们想要的分区，一个安装了 Fedora 的 Linux LVM 分区(id 8e)。

现在，有了这些信息，我们就可以计算安装偏移量，如下所示:

*扇区大小(512) x 起始扇区(3328000)=****1703936000***

这意味着我们需要将偏移量为 170393600 的图像挂载到正确的位置。然而，如果我们这样做，它会失败:

```
$ **sudo mount -o loop,offset=1703936000 Fedora-Server-34-1.2.aarch64.raw /mnt/raw3** mount: /mnt/raw3: unknown filesystem type 'LVM2_member'.
```

要解决这个问题，我们需要一个像`kpartx`这样的工具。这个实用程序在`/dev/mapper`中创建了一个虚拟设备，我们可以将它作为真实设备来操作。运行以下命令以逐字添加设备:

```
$ **sudo kpartx -a -v Fedora-Server-34-1.2.aarch64.raw** 
add map loop0p1 (253:0): 0 1228800 linear 7:0 2048
add map loop0p2 (253:1): 0 2097152 linear 7:0 1230848
add map loop0p3 (253:2): 0 11352064 linear 7:0 3328000
```

然后在`/dev/mapper`中验证新分区:

```
$ **ls -l /dev/mapper/**
crw-------. 1 root root 10, 236 May 14 11:20 control
lrwxrwxrwx. 1 root root       7 May 14 18:16 fedora_fedora-root -> ../dm-3
lrwxrwxrwx. 1 root root       7 May 14 18:16 loop0p1 -> ../dm-0
lrwxrwxrwx. 1 root root       7 May 14 18:16 loop0p2 -> ../dm-1
lrwxrwxrwx. 1 root root       7 May 14 18:16 loop0p3 -> ../dm-2
```

最后，创建一个目录并将 LVM 分区挂载到新目录:

```
sudo mkdir /mnt/raw3
sudo mount /dev/fedora_fedora/root /mnt/raw3
```

# 步骤 3 —直接在图像中工作

是时候开始修改图像了。为此，我们将使用 chroot 将会话的工作根更改为映像中的根。为了让这种模拟在不同的体系结构之间工作，我们需要安装`qemu-user-static`并重启`systemd-binfmt.service`:

```
sudo dnf install qemu-user-static
sudo systemctl restart systemd-binfmt
```

Chroot 到挂载的磁盘映像并启动一个 shell:

```
sudo chroot /mnt/raw3 /bin/bash
```

确保架构是 **aarch64** 而不是 x86_64:

```
# **uname -a**
Linux fedora 5.11.19-200.fc33.x86_64 #1 SMP Fri May 7 14:10:27 UTC 2021 aarch64 aarch64 aarch64 GNU/Linux
```

现在设置一个本地用户，我们稍后将使用它通过 SSH 进行连接。以下命令创建一个名为 *pi* 的组和用户。然后用户将获得 1000 的 UID，并被分配到 *pi* 和 *wheel* 组:

```
groupadd pi
useradd -g pi -G wheel -m -u 1000 pi
```

创建`.ssh`目录和`authorized_keys`文件，并分配适当的权限:

```
mkdir /home/pi/.ssh
chmod 700 /home/pi/.ssh
touch /home/pi/.ssh/authorized_keys
chmod 600 /home/pi/.ssh/authorized_keys
chown -R pi.pi /home/pi/.ssh/
```

然后将您的 Fedora 机器公钥(`~/.ssh/id_rsa.pub`)添加到`authorized_keys`文件中。

由于我们的新用户属于*轮*组，允许该组使用 sudo 而无需密码:

```
echo "%wheel ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/wheel-nopasswd
```

此外，Fedora 会在第一次启动时提示您完成设置。为了避免这种情况，请禁用初始设置:

```
unlink /etc/systemd/system/multi-user.target.wants/initial-setup.service
unlink /etc/systemd/system/graphical.target.wants/initial-setup.service
```

这就是我们需要改变的。退出 chroot 并卸载磁盘映像:

```
exit
sudo umount /mnt/raw3
```

现在我们需要删除 *loop0* 设备，但它包含一个 Linux LVM 分区:

```
$ **sudo pvscan**
  PV /dev/mapper/loop0p3   VG fedora_fedora   lvm2 [5.41 GiB / 0    free]
  Total: 1 [5.41 GiB] / in use: 1 [5.41 GiB] / in no VG: 0 [0   ]
```

因此，停用卷组 **fedora_fedora** 中的逻辑卷:

```
$ **sudo vgchange --activate n fedora_fedora**
0 logical volume(s) in volume group “fedora_fedora” now active
```

然后删除虚拟设备:

```
$ **sudo kpartx -d Fedora-Server-34-1.2.aarch64.raw**
loop deleted : /dev/loop0
```

# 步骤 4 —安装修改后的映像

剩下的就是两项耗时的任务:图像压缩和在 microSD 卡上安装。

将原始磁盘映像压缩到一个`.xz`文件，但这次保留`.raw`文件:

```
xz --compress Fedora-Server-34-1.2.aarch64.raw --keep
```

如果您想在将来进行增量更改，我建议您保留原始文件。否则，移除`--keep`参数。

> 此命令需要一段时间。

最后，将修改后的磁盘映像复制到 microSD 卡:

```
sudo arm-image-installer --image=Fedora-Server-34-1.2.aarch64.raw.xz --media=/dev/sdb --target=rpi3 --norootpass --resizefs -y
```

对于树莓派 4 来说，也是同样的命令，只是把目标替换成`--target=rpi4`。

> 这个命令也需要一些时间。

# 第 5 步—验证

让我们确认一下我们可以访问我们的新机器。将 microSD 卡插入您的 Raspberry Pi，并等待几分钟准备就绪。然后在你的 Fedora 机器(或任何其他连接到你家庭网络的电脑)上运行`nmap`或类似的工具，找出分配给你的 Raspberry Pi 的动态 IP 地址(相应地替换你的 CIDR 系列):

```
$ **sudo nmap -sn 192.168.0.0/24**
Nmap scan report for 192.168.0.126
Host is up (0.000078s latency).
MAC Address: E4:5F:01:AA:BB:CC (Raspberry Pi Trading)
```

在 MAC 地址中查找*树莓 Pi Trading* 或*树莓 Pi Foundation* 。然后使用您创建的 IP 地址和用户 SSH 到您的 Raspberry Pi:

```
$ **ssh pi@192.168.0.126**
The authenticity of host '192.168.0.126 (192.168.0.126)' can't be established.
ECDSA key fingerprint is SHA256:zx....
ECDSA key fingerprint is MD5:13:ce:...
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.0.126' (ECDSA) to the list of known hosts.
Web console: https://fedora:9090/ or [https://192.168.0.126:9090/](https://192.168.0.126:9090/)[pi@fedora ~]$
```

恭喜你！👏您已成功连接到您的 Raspberry Pi 上的新 Fedora 服务器！

一如既往，立即更新您的系统:

```
[pi@fedora ~]$ **sudo dnf update -y**
```

# [可选] —设置 Wi-Fi

默认情况下，Wi-Fi 在 Fedora 服务器中不能开箱即用，因为不包含`wpa_supplicant`。事实上，自 2019 年以来，有一个开放的 [Bugzilla 门票](https://bugzilla.redhat.com/show_bug.cgi?id=1756488)。

要解决此问题，请安装缺少的软件包，启用它并重新启动计算机:

```
[pi@fedora ~]$ **sudo dnf install wpa_supplicant -y**
[pi@fedora ~]$ **sudo systemctl enable wpa_supplicant**
[pi@fedora ~]$ **sudo shutdown -r now**
```

> 如果不重新启动 Raspberry Pi，Wi-Fi 将无法工作。

一旦 Raspberry Pi 恢复运行，列出可用的网络:

```
[pi@fedora ~]$ **nmcli device wifi list**
```

在列表中找到您的网络并连接到它:

```
[pi@fedora ~]$ **sudo nmcli device wifi connect <YOUR_SSID> --ask**
Password: ••••••••
Device 'wlan0' successfully activated with '15b5-XXX-XXX-XXX-495'.
```

现在，如果您列出设备状态，您会看到 Wi-Fi 已连接:

```
[pi@fedora ~]$ **nmcli device**
DEVICE         TYPE      STATE         CONNECTION
eth0           ethernet  connected     Wired connection 1
wlan0          wifi      connected     YOUR_SSID
```

# 包扎

我希望这个指南是有帮助的。现在你应该已经有了一个安装了 Fedora 的可用的 Raspberry Pi。尽管它是为 Fedora 设计的，但是您可以将这个过程应用于其他 Linux 发行版。如果您在前进的道路上发现任何障碍，或者如果您有任何问题/意见，请联系我，我会帮助您。享受你的圆周率！

# 参考

*   [架构/ARM/Raspberry Pi【Fedora Wiki】](https://fedoraproject.org/wiki/Architectures/ARM/Raspberry_Pi?rd=Raspberry_Pi#aarch64_supported_images_for_Raspberry_Pi_3)
*   [覆盆子 Pi 上的 Fedora【Fedora Docs】](https://docs.fedoraproject.org/en-US/quick-docs/raspberry-pi/)
*   [如何修改定制 Linux 发行版的原始磁盘映像](https://www.linux.com/news/how-modify-raw-disk-image-your-custom-linux-distro/)