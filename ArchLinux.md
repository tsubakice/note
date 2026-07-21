## 0. 基础软件包

```bash
pacstrap -K /mnt base base-devel linux-zen linux-firmware intel-ucode btrfs-progs bash-completion zram-generator neovim iwd
```

## 1. systemd-boot 引导

创建内核参数文件：`/etc/kernel/cmdline`，写入下述内核参数，参数 PARTUUID 可从 blkid 的输出中获取。

```cmdline
root=PARTUUID= rootfstype=btrfs rootflags=subvol=@ rw modprobe.blacklist=nouveau zswap.enabled=0
```

编辑引导配置文件，修改部分配置。

```bash
nvim /efi/loader/loader.conf # 设置启动菜单延时和默认引导策略
nvim /etc/mkinitcpio.d/linux-zen.preset # 启用默认预设
rm /boot/initramfs-linux-zen.img # 删除 initramfs
mkinitcpio -P # 重建引导
```

## 2. 交换空间

创建 zram-generator 配置文件：`/etc/systemd/zram-generator.conf`，并写入下述配置。

```conf
[zram0]
zram-size = ram / 2
compression-algorithm = zstd
swap-priority = 100
fs-type = swap
```

## 3. 普通用户

```bash
useradd -G wheel -m tsubaki
passwd tsubaki
EDITOR=nvim visudo
```

## 4. 配置网络

```bash
networkctl list # 查看网卡
sudo systemctl enable --now systemd-networkd systemd-resolved iwd
sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
sudo cp /usr/lib/systemd/network/89-ethernet.network.example /etc/systemd/network/89-ethernet.network # 创建有线网络连接配置
sudo cp /usr/lib/systemd/network/80-wifi-station.network.example /etc/systemd/network/80-wifi-station.network # 创建无线网络连接配置
sudo networkctl reload # 重载网络配置
```

## 5. 登录管理器

```bash
sudo pacman -S ly
sudo systemctl disable getty@tty1.service
sudo systemctl enable ly@tty1.service
```

## 6. 显卡驱动

### 6.1. intel 核显驱动

```bash
sudo pacman -S mesa vulkan-intel intel-media-driver
```

### 6.2. nvidia 独显驱动

```bash
sudo pacman -S linux-zen-headers dkms nvidia-open-dkms nvidia-utils nvidia-prime
```

编辑 mkinitcpio 配置文件：`/etc/mkinitcpio.conf`，并追加下述配置。

```conf
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

可选的，从 HOOKS 中移除 kms，并重新生成内核引导，确保内核不会加载 nouveau 驱动。

```bash
sudo mkinitcpio -P # 别忘了重建引导
```

## 7. 图形化环境

### 7.1. hyprland 窗口管理器

```bash
sudo pacman -S uwsm libnewt hyprland hyprlauncher foot qt5-wayland qt6-wayland noto-fonts noto-fonts-emoji adobe-source-han-sans-cn-fonts
mkdir -p ~/.config/uwsm # 创建 uwsm 配置目录存放环境变量
echo 'export LANG=zh_CN.UTF-8' > ~/.config/uwsm/env
```

### 7.2. nerd 字体

```bash
mkdir ~/.local/share/fonts # 创建用户字体目录
# ... # 将下载的 nerd 字体放入用户字体目录
fc-cache -v # 刷新字体缓存
```

### 7.3. hyprpolkitagent 权限提升代理

```bash
sudo pacman -S hyprpolkitagent
systemctl --user enable --now hyprpolkitagent
```

### 7.4. hyprpaper 壁纸

```bash
sudo pacman -S hyprpaper
```

创建 hyprpaper 配置文件：`~/.config/hypr/hyprpaper.conf`，并写入下述配置。

```conf
wallpaper {
    monitor = eDP-1
    path = ~/.wallpaper.png
    fit_mode = cover
}
```

在用户空间下启用 hyprpaper 服务。

```bash
systemctl --user enable --now hyprpaper
```

### 7.5. hyprcursor 光标

创建用户光标目录，然后将下载的光标放入该目录。

```bash
mkdir ~/.local/share/icons
```

创建 hyprland 环境变量文件：`~/.config/uwsm/env-hyprland`，并写入下述变量。

```bash
export HYPRCURSOR_THEME=下载的光标
export HYPRCURSOR_SIZE=24
```

































