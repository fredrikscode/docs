# Setup Arch Linux


## Pre-checks and tasks
1. Verify that we've booted into EFI mode: `efivar -L`
2. Set keymap. For Swedish: `loadkeys sv-latin1`
3. Ensure internet access through ethernet or WiFi.
    1. To connect to WiFi, use: `wifi-menu`
4. Enable NTP: `timedatectl set-ntp true`

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