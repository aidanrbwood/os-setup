# A basic setup guide for whenever I'm setting up a new OS

# Early setup
- If I'm formatting drive, format them in ext4

# Install nvim via AppImage

```
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage
chmod u+x nvim-linux-x86_64.appimage
sudo mv ./nvim-linux-x86_64.appimage /usr/bin/nvim
```

# Change the default terminal to bash

```
chsh -s /usr/bin/bash
```

Restart terminal

# Setup bash aliases

Copy the `bash_aliases` file in this repo to `~/.bash_aliases`
Copy the `bashrc` file in this repo to `~/.bashrc`

Restart terminal, might crash until keychain is installe and the ssh keys for github are setup

# Setup nvim

Setup the config directory and file for nvim

```
mkdir ~/.config/nvim
```

Copy in the contents of the `init.lua` file in this repo to `~/.config/nvim/init.lua`

# Config .inputrc

Copy the `.inputrc` file in this repo to `~/.inputrc` and then restart your terminal

# Config github keys

Install keychain

```
sudo pacman -Syu keychain
```

```
ssh-keygen githubs_keys
```

Go to github account and then setup pub key there, the `.bashrc` already contains the lines for adding the file to keychain

# Setup fstab to permanently mount smb shares

Make a new mount point

```
sudo mkdir /mnt/<MY_MOUNT_POINT>
```

Install cifs-utils

```
sudo pacman -S cifs-utils
```

Create a credentials file at `~/.smbcredentials`, fill out the contents as follows

```
username=<MY_USER>
password=<MY_PASSWORD>
domain=WORKGROUP
```

Add the following line to `/etc/fstab`

```
//<MY_IP_ADDR>/<MY_SHARE_NAME> /mnt/<MY_MOUNT_POINT> cifs credentials=/home/<MY_USER>/.smbcredentials,iocharset=utf8,uid=1000,gid=1000,file_mode=0775,dir_mode=0775,_netdev  0  0
```

# Setup fstab to mount any other non-boot drives

Make a new mount point

```
sudo mkdir /mnt/<MY_MOUNT_POINT>
```

Run the following to get the drive UUID

```
lsblk -f
```

Add the following line to `/etc/fstab`

```
UUID=<MY_UUID> /mnt/<MY_MOUNT_POINT> ext4 defaults,noatime 0 2
```
