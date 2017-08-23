# Antergos Community Edition (antiso)
Modified version of archiso to build Antergos-Deepin (livecd)

## Dependencies
- antergos-gfxboot for a graphical boot (or isolinux/syslinux)
- arch-install-scripts
- cpio
- dosfstools
- gfxboot
- libisoburn
- mkinitcpio-nfs-utils
- make
- opendesktop-fonts
- patch
- squashfs-tools
- wget

## Free space

Please, check that you have 5GB (or more) of free harddisk space in your root partition:
`df -h /`

## Instructions (without docker) 

1. Install dependencies:
```
sudo pacman -S arch-install-scripts cpio dosfstools gfxboot libisoburn mkinitcpio-nfs-utils make patch squashfs-tools wget
```
2. Clone this repository using `--recursive` like this:
```
git clone https://github.com/keeganmilsten/Antergos-Community-Editions.git --recursive
```

4. Install our modified mkarchiso and configurations by running:
```
cd Antergos-Community-Editions
sudo make install
```

5. While inside the `antergos-iso` folder, clone antergos-gfxboot and use antergos-gfxboot `colors` branch :
```
git clone https://github.com/antergos/antergos-gfxboot
cd Antergos-Community-Editions/antergos-gfxboot/
git checkout colors
```

6. Create `/work` and `/out` destination folders:
```
cd
sudo mkdir /work
sudo mkdir /out
```

The `/work` folder will store the livecd filesystem while the `/out` folder will store your new ISO file.

7. Go to the `config` directory you wish to build from.
- The "official" iso is in the `antergos` folder.
```
cd /home/user/antergos-iso/configs/antergos
```

8. Check text configuration file `config` with your favourite text editor.

9. Download openfonts package by heading here:

https://www.archlinux.org/packages/community/any/opendesktop-fonts/download/ 

Next, copy this to Antergos-Community-Editions/configs/deepin (or whatever version you want to build)

10. Clone the antergos iso-hotfix-utility:
```
cd Antergos-Community-Editions/configs/deepin/
sudo git clone https://github.com/antergos/iso-hotfix-utility
```

11. Copy the plymouth config files to deepin:
```
cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouth.initcpio_hook /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouth.initcpio_install /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouthd.conf /home/$USER/Antergos-Community-Editions/configs/deepin
```

12. Copy pacman files to deepin:
```
cp /home/$USER/Antergos-Community-Editions/configs/antergos/etc-pacman.d-gnupg.mount /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/pacman-init.service /home/$USER/Antergos-Community-Editions/configs/deepin
```

13. Copy xorg directory to deepin:
```
cp -R /home/$USER/Antergos-Community-Editions/configs/antergos/root-image/etc/antergos/xorg /home/$USER/Antergos-Community-Editions/configs/deepin
```

15. Copy isolinux directory to deepin:
```
cp -R /usr/share/Antergos-Community-Editions/configs/antergos/isolinux /home/$USER/Antergos-Community-Editions/configs/deepin
```

16. Build the iso:
```
sudo ./build.sh build
```

 **If you want to try to build the iso again, please remember to clean all generated files first:** 
 ```
 sudo ./build.sh clean
 ```

## Instructions (with Docker)

#### Install docker and setup your user
1. Install docker if you don't have it installed yet: 
```
sudo pacman -S docker
```
2. Add your user to the docker group (change USER for your username):
```
sudo usermod -aG docker USER
```

#### Setup docker images and container
1. Clone this repository :
```
git clone https://github.com/antergos/antergos-iso.git
```
2. Enter into `antergos-iso` folder and change to the `testing` branch:
```
cd antergos-iso
git checkout testing
```
3. Go into `docker` folder:
```
cd docker
```
4. Create `antergos-base` and `antergos-iso` docker images: 
```
sudo ./build-docker-images
```
5. Run a container based on the `antergos-iso` image:
```
docker run -it --mount source=outvol,target=/out --mount source=workvol,target=/work --name antergos-iso-build antergos-iso
```

#### Once inside the container, create the iso image
1. Go to `/antergos-iso` folder :
```
cd /antergos-iso
```
2. Go to the `config` directory you wish to build from.
- The "official" iso is in the `antergos` folder.
```
cd /home/user/antergos-iso/configs/antergos
```
3. Check text configuration file `config` with your favourite text editor
4. Build the iso: 
```
sudo ./build.sh build
```

**If you want to try to build the iso again, please remember to clean all generated files first:** 
 ```
 sudo ./build.sh clean
 ```
