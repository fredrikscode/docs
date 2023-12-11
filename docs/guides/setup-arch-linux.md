# Setup Arch Linux

1. Verify that we've booted into EFI mode: `efivar -L`
2. Set keymap. For Swedish: `loadkeys sv-latin1`
3. Ensure internet access through ethernet or WiFi.
    1. To connect to WiFi, use: `wifi-menu`
4. Enable NTP: `timedatectl set-ntp true`
5. Sort mirrors by rate: `reflector -c Sweden --sort rate --save /etc/pacman.d/mirrorlist`
6. sed -i '//#ParallelDownloads = 5/s/^#/g' /etc/pacman.conf
6. Sync mirrors: `pacman -Syyy`
7. Setup 500MB EFI, Whatever (5GB) swap, rest is a linux filesystem partition
8. mkfs.fat -F32 /dev/vda1
9. mkswap /dev/vda2
10. swapon /dev/vda2
11. mkfs.btrfs /dev/vda3
12. mount /dev/vda3 /mnt
13. Create subvolumes:
- btrfs su cr /mnt/@
- btrfs su cr /mnt/@home
- btrfs su cr /mnt/@root
- btrfs su cr /mnt/@log
- btrfs su cr /mnt/@cache
- btrfs su cr /mnt/@tmp
14. umount /mnt
15. Create directories
- mkdir /mnt/home
- mkdir /mnt/root
- mkdir /mnt/tmp
- mkdir -p /mnt/var/log
- mkdir -p /mnt/var/cache
15. Mount subvolumes
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@ /dev/vda3 /mnt
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@home /dev/vda3 /mnt/home
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@home /dev/vda3 /mnt/root
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@home /dev/vda3 /mnt/var/log
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@home /dev/vda3 /mnt/var/cache
- mount -o defaults,noatime,compress=zstd,commit=120,subvol=@home /dev/vda3 /mnt/tmp
16. mkdir -p /mnt/boot/efi
17. mount /dev/vda1 /mnt/boot/efi
18. pacstrap /mnt base base-devel linux linux-firmware vim btrfs-progs intel-ucode
19. genfstab -U /mnt >> /mnt/etc/fstab
20. arch-chroot /mnt
21. ln -sf /usr/share/zoneinfo/Europe/Stockholm /etc/localtime
22. hwclock --systohc
23. Enable en_US.UTF-8: vim /etc/locale.gen /// locale-gen
24. locale-gen
25. echo LANG=en_US.UTF-8 > /etc/locale.conf
26. echo KEYMAP=sv-latin1 > /etc/vconsole.conf
27. echo archbox > /etc/hostname
28. echo "127.0.0.1     localhost" >> /etc/hosts
29. echo "::1           localhost" >> /etc/hosts
30. echo "127.0.0.1     archbox.localdomain archbox
31. pacman -S networkmanager
32. systemctl enable NetworkManager
33. passwd for root
34. pacman -S grub efibootmgr
35. grub-install --target=x86_64-efi --efi-directory=/boot/efi
36. grub-mkconfig -o /boot/grub/grub.cfg
37. umount -R /mnt
38. reboot
39. useradd -m -g users -G audio,video,network,wheel,storage,rfkill -s /bin/bash fredrik
40. passwd fredrik
41. EDITOR=vim visudo --- uncomment wheel stuff
42. login as fredrik
43. sudo vim /etc/pacman.conf - Fix paralleldownloads
44. pacman -S xorg-server xorg-apps xorg-xinit alacritty sddm

## :material-harddisk: Partitioning

1. Run gdisk on the disk you're installing to: `gdisk /dev/sdX`
2. Type `x` then `Enter`, `z` then `Enter` and choose to proceed.
3. If the question "Blank out MBR?" comes up then choose `yes`.
4. Run `cgdisk /dev/sdX`

| Partition | Size    | Type | Name |
| --------- | ------- | ---- | ---- |
| `/boot`   | 1024Mib | EF00 | boot |
| `swap`    | 16GiB   | 8200 | swap |
| `/`       | 100GiB   | 8300 | root |
| `/home`   | 

| Method      | Description                          |
| ----------- | ------------------------------------ |
| `GET`       | :material-check:     Fetch resource  |
| `PUT`       | :material-check-all: Update resource |
| `DELETE`    | :material-close:     Delete resource |



## :octicons-package-24: Packages
- Base installation
    - **os-prober**: Spinoff of debian-installer. One feature is that it can probe disks for other operating systems and add them to the bootloader.
    - **xorg**: Display server, graphical output such as renedering GUI.
    - **xorg-xrdb**: To enable autostarting with Awesome.
    - **dex**: Same as above
    - **yay**:
        - sudo git clone https://aur.archlinux.org/yay-git.git /opt
        - sudo chown -R user:group /opt/yay-git
        - cd /opt/yay-git
        - makepkg -si
    - **nvidia/nvidia-lts**
    - **nitrogen**
    - **mtools** (?): A collection of utilities to access MS-DOS disks from GNU and Unix without mounting them
    - **lvm2**
    - **linux/linux-lts**
    - **linux-headers/linux-lts-headers**
    - **intel-ucode/amd-ucode**
    - **efibootmgr**
    - **dosfstools**
    - **base**
    - **base-devel**
    - **xorg-xrandr**
    - **zsh**
    - **zsh-completions-git (AUR)**
    - **neovim**
    - **brave-bin (AUR)**
    - **stow** - Config management of dotfiles
    - **imagemagick** - CLI screenshots
    - **betterlockscreen (AUR)**
    - **polybar-spotify-module (AUR)**
    - **snapper** - BTRFS subvolume management

- LightDM
    - **lightdm**
    - **lightdm-gtk-greeter**
- Awesome WM
    - **awesome-git (AUR)**

- Customization/Theming/Utilities
    - **rofi**
    - **rofi-emoji**
    - **rofi-calc**
    - **picom-jonaburg-git** (which version? animations are cool)
    - **powerlevel10k (AUR)**
    - **dunst** + **libnotify**

- Just apps
    - **caprine** (messenger)

- spotify keys


- Change default shell:
    - chsh -s /bin/zsh

- Check keys for keybinding:
    - sudo showkey -a (ctrl+D to exit)

- Check rc.lua for config errors:
    - awesome -k