# zenbook-issues-with-ubuntu18.04

My solution on issues about asus zenbook Ux533 with ubuntu 18.04.

Ux533 is a good laptop, but I guess it is too new for ubuntu 18.04

## 0. Install ubuntu 
Frequent crash while installing ubuntu.

It is the problem about the graphic card.

Choose install Ubuntu option (BUT DONT PRESS ENTER)

Press e

Find the line that starts with linux then add 

    modprobe.blacklist=nouveau

after quiet splash.

Once the installation is done, remove the usb and reboot system

Make sure security boot is disabled (in BIOS)

Boot, select Ubuntu at GRUB (repeat modprobe.blacklist=nouveau step if the screen freezes again).

Once booted successfully, blacklist nouveau permanently

    sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

    sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

    cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

You should see:

    blacklist nouveau

    options nouveau modeset=0

then 

    sudo update-initramfs -u

to activate

then install nvidia driver, 

    sudo add-apt-repository ppa:graphics-drivers/ppa

    sudo apt update

    sudo apt install nvidia-driver-xxx (418 or 410 or...)

    sudo reboot

once booted
    
    nvidia-smi

to see if you have driver or not, see issue 2 if got purple screen after reboot (especially after update and upgrade)

## 1.speaker no sound or keyboard backlight

If no sound from speaker/keyboard backlight notworking but works fine under windows:

reason: kernel 4.15 does not support hardware

solution: update kernel to 4.20

my way:

install ukuu (https://www.omgubuntu.co.uk/2017/02/ukuu-easy-way-to-install-mainline-kernel-ubuntu)

    sudo add-apt-repository ppa:teejee2008/ppa

    sudo apt-get update && sudo apt-get install ukuu
  
then open ukuu in application (show application, the 3*3 dots icon left lower corner, or use window key, to search ukuu)

then upgrade kernel to 4.20 or higher (4.19.x may work, but 4.19.0 does not,i guess somewhere beween 4.19.1 and 4.20) 


## 2. boot stuck in purple screen after update or loading initial ramdisk in recovery mode

after update then boot stuck in purple screen and stuck in loading initial ramdisk with recovery mode

reason: intel update

solution: 

in grub, select ubuntu, DO NOT PRESS ENTER ,presse 'e', add   

    dis_ucode_ldr  
    
after the line that starts with linux, then boot (press F10)

once booted, in terminal: 

    sudo apt install intel-microcode=3.20180312.0~ubuntu18.04.1 

then hold package:  

    sudo apt-mark hold intel-microcode=3.20180312.0~ubuntu18.04.1

should solve the problem

## 3. cuda 10.1 install 

it sucks, as usual.

1. if not boot after install 

in grub, select ubuntu, DO NOT PRESS ENTER ,presse 'e', add   

    modprobe.blacklist=nouveau  
    
to the end of the line that starts with linux (after splash), then boot,

if bootable, then step 2

2. blacklist nouveau

        sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

        sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

        cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

You should see:

    blacklist nouveau

    options nouveau modeset=0

then 

    sudo update-initramfs -u

then install cuda driver, see step 3

3. install cuda 

deb file (with driver) works for me, the following is by my personal 5 years cuda installation summary 

download deb file and install should work, if not, 

    sudo apt-get purge --remove cuda*
    sudo apt-get install ubuntu-desktop

if the driver is loaded (after reboot), then install through cuda.run file without driver.

if not, then install cuda driver, 

    sudo apt-get purge --remove nvida*
    sudo apt-get install ubuntu-desktop
    sudo add-apt-repository ppa:graphics-drivers/ppa
    sudo apt update
    sudo apt install nvidia-driver-xxx (418 for cuda 10, 410 for cuda 9, ...)
    sudo reboot

once rebooted

    nvidia-smi
    
see if driver was installed properly or not, if it does, then 

Then download cuda.run file and install cuda without install driver,

If it is still not working, well, a clean install ubuntu may help.

## 4. Turn off screen 

in terminal: 

    xset dpms force off

permanent: add

    alias screenoff='xset dpms force off' 
    
to .bashrc

then in the futurn, just type 'screenoff' in the terminal 

update : just use fn + f4 multiple times

## 5. Slow wifi
resaon : 18.04 will use 802.11n but the router may use a/b/g

Check if it works first with:

    sudo modprobe -r iwlwifi

    sudo modprobe iwlwifi 11n_disable=1

Make it permanent with this command:

    echo "options iwlwifi 11n_disable=1" | sudo tee /etc/modprobe.d/iwlwifi.conf

## 6. Fn+Esc not working
Fn+Esc is not working out of the box, meaning that Fn keys cannot be locked. This can be easily fixed by updating to kernel version 5.2.3 or higher. See issue #1 for instructions on how to update the kernel.

### Disclaimer

It is just problem I have with my zenbook, if it destroys your system, well, das Leben ist kein Ponyhof.

If you have other problems, I may be able to help (if the system is not fresh installed, 

then the possibitties will be way to high)

my email: a36402191234@gmail.com
