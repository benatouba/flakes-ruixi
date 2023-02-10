**简体中文（中国大陆）** | [English (UK)](README_en.md)

[![Push Blog](https://github.com/ruixi-rebirth/flakes/actions/workflows/push_blog.yml/badge.svg)](https://ruixi-rebirth.github.io) 

<p align="center"><img src="https://user-images.githubusercontent.com/75824585/210402874-da3422d5-ab65-4975-b73a-c300065c6792.png" width=300px></p>
<h2 align="center">Ruixi-rebirth's NixOS Config</h2>
<p align="center"><img src="https://user-images.githubusercontent.com/75824585/196195007-ecebb290-2c6b-4fab-9e1e-2dbb12f7eb44.png" width=300px></p>

https://user-images.githubusercontent.com/75824585/201473117-578af0df-e4ea-4dc9-91a6-c30281d46e7a.mp4

### 系统组件
||NixOS(Wayland)|
| - | :--: |
|**Window Manager**|[Sway](https://github.com/swaywm/sway), [Hyprland](https://github.com/hyprwm/Hyprland)|
|**Terminal Emulator**|[Kitty](https://github.com/kovidgoyal/kitty)|
|**Bar**|[Waybar](https://github.com/Alexays/Waybar)|
|**Application Launcher**|[Rofi-wayland](https://github.com/lbonn/rofi)|
|**Notification Daemon**|[Mako](https://github.com/emersion/mako)|
|**Display Manager**|None(TTY1 Login)|
|**network management tool**|[NetworkManager](https://networkmanager.dev/)|
|**Input method framework**|[Fcitx5](https://github.com/fcitx/fcitx5)|
|**System resource monitor**|[Btop](https://github.com/aristocratos/btop)|
|**File Manager**|[Ranger](https://github.com/ranger/ranger), [Nemo](https://github.com/linuxmint/nemo)|
|**Lockscreen**|[Swaylock-effects](https://github.com/mortie/swaylock-effects)|
|**Shell**|[Fish](https://github.com/fish-shell/fish-shell)|
|**Music Player**|[mpd](https://github.com/MusicPlayerDaemon/MPD), [ncmpcpp](https://github.com/ncmpcpp/ncmpcpp), [mpc](https://github.com/MusicPlayerDaemon/mpc), [Netease-cloud-music-gtk](https://github.com/gmg137/netease-cloud-music-gtk)|
|**Media Player**|[mpv](https://github.com/mpv-player/mpv)|
|**Text Editor**|[Neovim](https://github.com/neovim/neovim)|
|**Icons**|[Papirus](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme)|
|**Fonts**|[Nerd fonts](https://github.com/ryanoasis/nerd-fonts)|
|**Image Viewer**|[imv](https://sr.ht/~exec64/imv/)|
|**Screenshot Software**|[grimshot](https://github.com/swaywm/sway/blob/master/contrib/grimshot),[grimblast](https://github.com/hyprwm/contrib)|
|**Screen Recording**|[wf-recorder](https://github.com/ammen99/wf-recorder), [OBS](https://obsproject.com)|
|**Clipboard**|[wl-clipboard](https://github.com/bugaevc/wl-clipboard)|
|**Color Picker**|[hyprpicker](https://github.com/hyprwm/hyprpicker)|

### 主题
**catppuccin-light**
![2023-01-12T02:21:59](https://user-images.githubusercontent.com/75824585/211895195-e0a47165-e635-4256-922c-17d7da1ed62e.png)

**catppuccin-dark**
![2023-01-12T03:00:29](https://user-images.githubusercontent.com/75824585/211895280-41d12bfe-453c-41da-a2a6-3f7f483ea8ad.png)

**nord**
![2023-01-17T00:15:22](https://user-images.githubusercontent.com/75824585/212723937-c56200da-52f4-407b-9d8e-428348ed5ed0.png)


*安装主题任意选择一个即可：具体见[这里](https://github.com/Ruixi-rebirth/flakes/blob/main/hosts/laptop/home.nix#L11-L13)*

### 屏幕截图
<details>
<summary><b>Click to expend</b></summary>

![](./screenshot/light.png)
![](./screenshot/dark.png)
![](./screenshot/nord.png)

</details>

### 目录结构
```
.
├── flake.lock
├── flake.nix
├── hosts
│   ├── default.nix
│   ├── laptop
│   └── system.nix
├── modules
│   ├── desktop
│   ├── devlop
│   ├── editors
│   ├── environment
│   ├── fonts
│   ├── hardware
│   ├── programs
│   ├── scripts
│   ├── shell
│   ├── theme
│   └── virtualisation
├── overlays
│   └── default.nix
├── pkgs
│   ├── catppuccin-cursors
│   ├── catppuccin-gtk
│   └── default.nix
├── README_en.md
├── README.md
└── screenshot
    └── screenshot.png
```

### 如何安装?(root on tmpfs)
0. 准备一个64位的nixos [minimal iso image](https://channels.nixos.org/nixos-22.11/latest-nixos-minimal-x86_64-linux.iso) 烧录好,然后进入live系统。假设我已经分好两个分区`/dev/nvme0n1p1` `/dev/nvme0n1p3`
1. 格式化分区
```bash
  mkfs.fat -F 32 /dev/nvme0n1p1 
  mkfs.ext4 /dev/nvme0n1p3
```
2. 挂载
```bash
  mount -t tmpfs none /mnt 
  mkdir -p /mnt/{boot,nix,etc/nixos}
  mount /dev/nvme0n1p3 /mnt/nix
  mount /dev/nvme0n1p1 /mnt/boot 
  mkdir -p /mnt/nix/persist/etc/nixos
  mount -o bind /mnt/nix/persist/etc/nixos /mnt/etc/nixos
```
3. 生成一个基本的配置 
```bash
  nixos-generate-config --root /mnt
```
4. 克隆仓库到本地
```bash
nix-shell -p git
git clone  https://github.com/Ruixi-rebirth/flakes.git /mnt/etc/nixos/Flakes 
cd  /mnt/etc/nixos/Flakes/
nix develop --extra-experimental-features nix-command --extra-experimental-features flakes
```
5. 将 /mnt/etc/nixos 中的 `hardware-configuration.nix` 拷贝到 /mnt/etc/nixos/Flakes/hosts/laptop/hardware-configuration.nix
```bash 
cp /mnt/etc/nixos/hardware-configuration.nix /mnt/etc/nixos/Flakes/hosts/laptop/hardware-configuration.nix
```
6. 修改被覆盖后的 `hardware-configuration.nix`
```bash
nvim /mnt/etc/nixos/Flakes/hosts/laptop/hardware-configuration.nix
```
```nix
...
#这只是一个例子
#请参考 `https://elis.nu/blog/2020/05/nixos-tmpfs-as-root/#step-4-1-configure-disks`

  fileSystems."/" =
    { device = "none";
      fsType = "tmpfs";
      options = [ "defaults" "size=8G" "mode=755"  ];
    };

  fileSystems."/nix" =
    { device = "/dev/disk/by-uuid/49e24551-c0e0-48ed-833d-da8289d79cdd";
      fsType = "ext4";
    };

  fileSystems."/boot" =
    { device = "/dev/disk/by-uuid/3C0D-7D32";
      fsType = "vfat";
    };

  fileSystems."/etc/nixos" =
    { device = "/nix/persist/etc/nixos";
      fsType = "none";
      options = [ "bind" ];
    };
...
```
7. 移除 '/mnt/etc/nixos/Flakes/.git'
```bash 
rm -rf .git
```
8. 如果你想要将默认用户名称从 `ruixi` 更改成为其他名称，请打开 `/mnt/etc/nixos/Flakes/flake.nix` 并修改其中的 `user` 变量为对应的名称。

9. 使用 `mkpasswd {PASSWORD} -m sha-512` 命令生成的密码哈希串替换掉 `/mnt/etc/nixos/Flakes/hosts/laptop/default.nix` 中的 `users.users.<name>.hashedPassword` 值替换掉。（在文件中有两处需要替换的内容）

10. 安装
```bash
nixos-install --no-root-passwd --flake .#laptop

#或者指定源：
nixos-install --option substituters "https://mirrors.tuna.tsinghua.edu.cn/nix-channels/store" --no-root-passwd --flake .#laptop
```

11. 重启
```bash
reboot
```
12. 享受它吧！
