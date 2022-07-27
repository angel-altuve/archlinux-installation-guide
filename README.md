# archlinux installation guide 

## first steps 

**Internet connection:**

**root@archiso ~ #**
```bash
ip link
```

Executing this command will show you various interfaces, i.e. devices, to connect to the Internet. Among them there should be one called “wlan0”.

Next we will have to activate this device to be able to use it, to activate it we execute the following command:

**root@archiso ~ #**
```bash
ip link set wlan0 up
```

If the device has been successfully activated then we can proceed. If it is not activated, it means that there was an error trying to activate the device.

To check if the device has been turned on, we execute the previous command again:

**root@archiso ~ #**

```bash
ip link
```

Remember that in my case it is “**wlan0**“, but it doesn't always have to be like that, that's why we have run **ip link** to check it, and you may have to adapt the commands according to the name of your device.

When doing so, it should appear next to the interface, that is, together with the device **“wlan0”**, the word **“UP”**.

Once we have the device turned on we will have to scan the Wi-Fi networks that we have nearby and connect to one of them.

To perform a scan of Wi-Fi networks we will have to execute the following command:

**root@archiso ~ #**
```bash
iwlist wlan0 scan
```

This command will show a complete list with all the Wi-Fi networks that we have nearby, as well as the name of each of them.

The name of the Wi-Fi network together with the password is what we will have to use next to connect.

## Connect to a Wi-Fi network with a WPA password

If the Wi-Fi network to which we want to connect uses the WPA or WPA2 encryption protocol, then we will have to execute two commands.

The first command is to configure the password that we are going to use and the ESSID, and we save it in a configuration file, for example **“/etc/wifi”**:

**root@archiso ~ #**
```bash
wpa_passphrase <WiFi network name> <passwd> > /etc/wifi
```

The file **«/etc/wifi»** that has been generated can be viewed with the command:

**root@archiso ~ #**
```bash
cat /etc/wifi
```

Next we will have to connect to this Wi-Fi network using the configuration file that we have generated, for this we will have to execute the following command:

**root@archiso ~ #**
```bash
wpa_supplicant -B -i wlan0 -D wext -c /etc/wifi
```

The command "**wpa_supplicant**" is used to connect to a Wi-Fi network that uses **WPA/WPA2** as the encryption method.

The **-B** argument indicates that the command will be executed in the background. This prevents the "log" from appearing on the command line.

The **-i** argument is used to specify the name of the Wi-Fi interface, in this case **"wlan0"**.

The argument **-D** indicates the "driver" or controller to use, in this case, I use **"wext"**.

Finally, the **-c** argument is used to indicate the configuration file to use, in this case, **«/etc/wifi»**, which we have generated with the previous command: **wpa_passphrase**.

## **End Wi-Fi connection**

Once we have made the connection, depending on the type of Wi-Fi network password, we will have to end the connection and establish an IP negotiation. This may seem very complicated, but it really is not, since we only have to execute a single command:

**root@archiso ~ #**
```bash
dhclient
```

The **dhclient** command is used to perform **IP** negotiation, that is, to obtain a local **IP** address and a **DNS** address automatically. Without this command, even being connected to the Wi-Fi network, we would not have an Internet connection.

Once this is done, we would already have a connection via Wi-Fi, which we can check in the same way as if we were using cable with the “**ping**” command:

**root@archiso ~ #**
``` bash
ping google.com
```

## **Partition disk:**

****To consult the partition table of your hard disk where you are going to install the Operating System, use the following command.

**root@archiso ~ #**
```bash
fdisk -l
```

## **HDD**

First we must identify what the path of our disks is like, then know which are partitions.

Disk path can be:

/dev/sda (sdd or hdd)

/dev/sdb (sdd or hdd)

/dev/sdc (This is how it changes letters...)

/dev/nvme0n1 (veMMC or SD Card)

/dev/mmcblk0 (veMMC or SD Card)
## lsblk

### **Create the partitions**

**>> Arch Linux includes the following partitioning tools:**


| Software               | MBR             |GPT
| ---------------------- | --------------  |-------------
| Dialog                 | fdisk, parted   |fdisk, cgdisk, 
| pseudo-graphics        | cfdisk          | cgdisk, cfdisk
| no-iterative           | sfdisk, parted  |sfdisk, sgdisk parted

**>> Linux file structure**

We must know that the other folders are born from **root**

The advantage that **Linux allows assigning a partition for each folder**

We can have **/boot/** in a partition

We can have **/home/** in another partition

**/home/** the HOME folder stores the files of all users something similar to a DISK D: of windows*

*It all depends if we want to have it separated in another partition*

Which can be good or bad, depends on the needs of the user

**Examples of partitions:**

## **>> SWAP Memory**

Less than 1GB physical RAM >> 2GB SWAP

Between 2GB to 4GB physical RAM >> 2GB to 4GB SWAP

8GB physical RAM >> 4GB SWAP

More than 8GB of physical RAM >> 2GB to 4GB of SWAP

Understood these concepts we execute **cfdisk**

**root@archiso ~ #**
```bash
 cfdisk /dev/sda
```

/*Let's start by deleting the partitions in **[ Delete ]** and creating new ones in **[ New ]**

/* We select **[ Bootable ]** where the Root partition is

/* We can create several partitions and they will only be generated: **sda1, sda2, sda3...**

/* We can create the partitions and change the partition type in **[ Type ]**

/***Up / Down Arrow - Right / Left Arrows** - to move in cfdisk

**In this example we will use:**

sda1 > Swap

sda2 > root

Sda3 > Home

/* At the end of the partitioning we give it in **[ Write ]** to write the changes

## **Format the partitions**

There are multiple partition formats available to use, however in this guide we will use ext4

Ext4 is the most used and recommended.

Ext4 is a Linux journaling file system.

Virtual memory partition or SWAP swap memory:

**root@archiso ~ #**
```bash
mkswap /dev/sda1
```

Format Root Partition into one partition:

**root@archiso ~ #**
```bash
mkfs.ext4 /dev/sda2
```

Format Home Partition into one partition:

**root@archiso ~ #**
```bash
mkfs.ext4 /dev/sda3
```

Activate SWAP partition:

**root@archiso ~ #**
```bash
swapon /dev/sda1
```

### **Mount the partitions**

Now for root in this example it is **/dev/sda2** which must be mounted first since everything starts with ***ROOT /***

***root@archiso ~ #***
```bash
mount /dev/sda2 /mnt/
```

To mount the partitions we need to first create the logical mount paths.

If you are going to install Archlinux as a single system in BIOS/Legacy mode, then we need to create ***/mnt*** and inside this directory include ***/boot*** or ***/home*** or * **/tmp*** or etc...

In this case we only have ***/home*** on /dev/sda3

***root@archiso ~ #***
```bash
mkdir -p /mnt/home/
```

Mounting the Boot partition ***/boot***

***root@archiso ~ #***
```bash
mount /dev/sda3 /mnt/home
```

We verify that the directories have been created correctly

***root@archiso ~ #***
```bash
ls /mnt/
```

> /mnt - manually mounted file systems on the hard drive.

> / - backslash means root

### **Mirrorlist generator inside LiveCD**

Now that the partitions are mounted, we install the base programs and the most essential as possible.

But there are many cases that pacstrap finds it a very slow download and that is due to not having the fastest download mirrors.

To have the fastest Mirrors to have better downloads we will use ***reflector***

***root@archiso ~ #***
```bash
pacman -Sy reflector python --noconfirm
```

To run reflector and have the best Mirrors Servers is:

***root@archiso ~ #***
```bash
reflector --verbose --latest 15 --sort rate --save /etc/pacman.d/mirrorlist
```

To review the list of Mirrors Servers and confirm that Reflector did it, the command is:

We confirm by comparing where it says ***by Reflector***

***root@archiso ~ #***
```bash
cat /etc/pacman.d/mirrorlist
```

## **System Installation**

**Specification of the necessary packages.**

***root@archiso ~ #***
```bash
pacstrap /mnt base base-devel nano
```

/* **Base installation - base-devel**: programs, settings, directories, etc...

/* **Text editor in terminal**: Nano

***root@archiso ~ #***
```bash
pacstrap /mnt linux-firmware linux linux-headers mkinitcpio
```

/* **linux-firmware**: Generally proprietary driver binaries, Modern graphics cards from AMD, NVIDIA and Intel Wi-Fi require blob loading for the hardware to function properly.

/* **linux**: kernel in its stable version

/* **linux-headers**: kernel headers in its stable version

/* **mkinitcpio**: Initramfs image creation utility for the kernel

After Kernel installation, Automatic Linux IMGs will be created in ***/boot/*** folder thanks to ***mkinitcpio***

**Specification of the necessary packages.**

The following packages allow us to manage Internet connections

***root@archiso ~ #***
```bash
pacstrap /mnt dhcpcd networkmanager net-tools wpa_supplicant dialog netctl
```
In case you use a laptop, for Bluetooth: (optional)

***root@archiso ~ #***
```bash
pacstrap /mnt bluez bluez-utils pulseaudio-bluetooth
```

If you get an error when using pacstrap of the type "error: could not open file /mnt/var/lib/pacman/sync/core.db: Unrecognized archive format" we have to remove the "...sync/" directory recursively in order to solve it.

***root@archiso ~ #***
```bash
rm -R /mnt/var/lib/pacman/sync/
```

### **The fstab file**

It is used to define how the partitions,

These definitions were dynamically mounted on boot

***root@archiso ~ #***
```bash
genfstab -p /mnt >> /mnt/etc/fstab
```

**/*After generating the fstab file for our partition labels**

**/*We check with:**

***root@archiso ~ #***
```bash
cat /mnt/etc/fstab
```

## **Chroot**

We enter the root of the new system as the root user.

***root@archiso ~ #***
```bash
arch-chroot /mnt
```

/* Enter root as root

/* We see change the color and change the **~** of the livecd by **/** which means root.

/* Within our system we are going to configure language, keyboard, time and users.

***[root@archiso /]#***
```bash
nano /etc/locale.gen
```

/* We remove the ***#*** which is a comment in our language >> ***en*** and our country

/*Must end in ***en_US.UTF-8***

Ctrl + W to search for words in nano

Ctrl + O to save to nano

Ctrl + X to close in nano

We generate the selected language

***[root@archiso /]#***
```bash
locale-gen
```

set the LANG variable in locale.conf
set the LC_CTYPE variable in locale.conf
set the LC_TIME variable in locale.conf
set the LC_COLLATE variable in locale.conf

***[root@archiso /]#***
```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

``` bash
echo LC_CTYPE=en_US.UTF-8 > /etc/locale.conf
```

``` bash
echo LC_TIME=en_US.UTF-8 > /etc/locale.conf
```

``` bash
echo LC_COLLATE=en_US.UTF-8 > /etc/locale.conf
```

Export the LANG variable with the locale specified

***[root@archiso /]#***
```bash
export LANG=en_US.UTF-8
```

***[root@archiso /]#***
```bash
ls /usr/share/zoneinfo/America/
```

***[root@archiso /]#***
```bash
ls /usr/share/zoneinfo/Europe/
```

/* **ln -sf** - Generates a symbolic link is an access to the file

***[root@archiso /]#***
```bash
ln -sf /usr/share/zoneinfo/America/<name of your area> /etc/localtime
```

## **User Configuration**

The system is configured to read the computer's internal clock, then the system clock

Set the RTC from the system time.

**[root@archiso /]#**
```bash
hwclock -w
```

Define the keyboard layout in vconsole.conf

so that it remains in each reboot only applies to the virtual terminal.

to keep it active on your Desktop (DE) is another command.

**[root@archiso /]#**
```bash
echo KEYMAP=us > /etc/vconsole.conf
```

Team name, this is not **USER!**

In **name_of_pc** they change it for one to your liking, **later on they will create a user**

**[root@archiso /]#**
```bash
echo <Pc_name> > /etc/hostname
```

We modify the file **Hosts**

It is important to know what name they put in **Hostname** because it will be used here

```bash
nano /etc/hosts
127.0.0.1 [tab] localhost
::1 [tab] localhost
127.0.1.1 [tab] <pc_name>.localdomain(space)<pc_name>
```

Ctrl + O to save to nano

Ctrl + X to close in nano

### **USERS**

Password for root:

**[root@archiso /]#**
```bash
passwd root
```

We create our user, to enter our system.

**[root@archiso /]#**
```bash
useradd -m -g users -G wheel -s /bin/bash <Username>
```

**[root@archiso /]#**
```bash
passwd <Username>
```

In username, we change it to our liking.

Now with our user if we want to do something as root we usually use SUDO

Sudo will take effect if our user is in the **Sudoers** list.

**[root@archiso /]#**
```bash
nano /etc/sudoers
```

We look for **root ALL=(ALL) ALL** and below we put our user

So that you have same permissions and privileges when running sudo.

**[root@archiso /]#**
```bash
<username> ALL=(ALL) ALL
```

Ctrl + W to search for words in nano

Ctrl + O to save to nano

Ctrl + X to close in nano

### **Activating Services**

Dynamic Host Configuration Protocol (DHCP)

The server provides clients with a dynamic IP address,

The subnet mask, Thanks to "systemd" we can activate that service

Automatic detection and configuration to connect to the network, respect capitalization

**[root@archiso /]#**
```bash
systemctl enable dhcpcd NetworkManager
```

If you installed Bluetooth packages activate the service:

**[root@archiso /]#**
```bash
systemctl enable bluetooth
```

### **Mirrorlist in root**

**Reflector** is a script that is capable of generating a list and uses the fastest repositories

Sort them based on their speed, and overwrite the **/etc/pacman.d/mirrorlist file**

**[root@archiso /]#**

```bash
 pacman -Sy reflector
```

**[root@archiso /]#**

``` bash
 reflector --verbose --latest 15 --sort rate --save /etc/pacman.d/mirrorlist
```

### **GRUB - Bootloader**

We install GRUB and test you

**Os-prober** detects more operating systems if we had them, which will use grub for its menu (optional)

```bash
pacman -S grub os-prober
```

Here it only matters to install grub on the disk where ArchLinux is installed, in my case it is **/dev/sda**

```bash
grub-install /dev/sda
```

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

```bash
mkinitcpio -p
```

### **Leaving LiveCD**

```bash
exit
```

```bash
exit
```

We exit the system, unmount all partitions...

```bash
umount -R /mnt
```

And finally we restart, ** we remove the USB or the CD when the pc is turned off **


```bash
reboot
```

### We verify which is our network card with the following command

```bash
ip link
```

**we activate our network card with the following command**

``` bash
sudo ip link set <"network card name"> up
```

**>> to see the available networks**

```bash
nmcli dev wifi list
```

If the network has spaces we just put single or double quotes in the bssid:

``` bash
sudo nmcli dev wifi connect <network name> password <password>
```

We check the connection:

```bash 
nmcli dev status
```
We are ready to start downloading in our system

**But always before doing any installation we always update the system with:**

```bash
sudo pacman -Syu
```

### **Customizing PACMAN**

The general pacman configuration file is located at:

```bash
sudo nano /etc/pacman.conf
```

We are located in the part that says: **# Misc options**

 ```bash
# Misc options
#UseSyslog
Color
#NoProgressBar
CheckSpace
VerbosePkgLists
ParallelDownloads = 5
ILoveCandy
```

And we add **ILoveCandy** (it's a capital i and then an L)

Then we remove the **#** comment on the **MultiLib** repository

This gives us several 32bit libraries, in old games or programs that need those libraries.

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```

We update the system to see the color and animation of pacman:

```bash
sudo pacman -Syu
```

### **Extra Programs**

```bash
sudo pacman -Sy git wget
```

/* Downloads with git

/* Download with wget

```bash
sudo pacman -Sy neofetch
```

```bash
sudo pacman -Sy intel-ucode
```

Processor manufacturers release stability and security updates for processor microcode **(closed source)**

We don't have the common directories:

Desktop

documents

downloads

Music

Images

Public

Templates

Videos

```bash
sudo pacman -S xdg-user-dirs
```

The way it works is that **xdg-user-dirs-update** is executed

Then create localized versions of these directories

In the users home directory with special icons

```bash
xdg-user-dirs-update
```

### **X.Org Server**

X.Org Server provides the standard tools to provide graphical interfaces

X.Org is required for both Desktop environment (DE) and Window manager (WM)

```bash
sudo pacman -Sy xorg xorg-apps xorg-xinit xorg-twm xterm xorg-xclock
```

To execute X.org

```bash
startx
```

### **Video driver**

With the following command it will give us information about our graphics card

```bash
lspci | grep VGA
```

**for Intel cards**

```bash
sudo pacman -S xf86-video-intel
```

**Audio**

```bash
sudo pacman -S pulseaudio pulseaudio-alsa alsa-mixer pavucontrol
```

### **letter fonts**

```bash
sudo pacman -S gnu-free-fonts ttf-hack ttf-inconsolata
```

### **Installation  paru**

First install git and base-devel Package Association which contains tools for creating (classifying and linking) packages from source code

```bash
sudo pacman -S --needed base-devel
```

clone paru repository:

```bash
git clone [https://aur.archlinux.org/paru.git](https://aur.archlinux.org/paru.git)
```

Change to paru Directory:

```bash
cd paru
```

Finally, create and install the Paru AUR helper on Arch Linux using the command:

```bash
makepkg -si
```

Supplemental Firmware

```bash
paru -S mkinitcpio-firmware
```

**go through my xmonad configuration**
[xmonad](https://github.com/angel-altuve/dotfiles)
