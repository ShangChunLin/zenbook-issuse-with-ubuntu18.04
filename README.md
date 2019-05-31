# zenbook-issuse-with-ubuntu18.04

My solution on issues about asus zenbook Ux533 with ubuntu 18.04.

Ux533 is a good laptop but I guess it is too new for ubuntu 18.04

## 0. Install ubuntu 
Frequcent crush while install ubuntu
I guess it is the problem about graphic card

Choose install Ubuntu option (BUT DONT PRESS ENTER)

Press e

Find the line that starts with linux then add 

    modprobe.blacklist=nouveau

after quiet splash.

Once the installation is done, remove the usb and reboot system

Make sure security boot is disable

Boot, select Ubuntu at GRUB (repeat modprobe.blacklist=nouveau step if the screen freezes again).

Once boot successfully, blacklist nouveau permanently

    sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

    sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

    cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

should see:

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

once bott
    
    nvidia-smi

see you have driver or not, see issue 2 if got purple screen after reboot (especially after update and upgrade)

## 1.speaker no sound or keyboard backlight

If no sound from speaker/keyboard backlight notworking but works fine under windows :

reason: kernel 4.15 does not support hardware

solution: update kernel to 4.20

my way:

install ukuu (https://www.omgubuntu.co.uk/2017/02/ukuu-easy-way-to-install-mainline-kernel-ubuntu)

    sudo add-apt-repository ppa:teejee2008/ppa

    sudo apt-get update && sudo apt-get install ukuu
  
then open ukuu in application (show application, the 3*3 dots icon left lower corner, or use window key, to search ukuu)

then upgrade kernel to 4.20 or higher (4.19.x may work, but 4.19.0 does not,i guess somewhere beween 4.19.1 and 4.20) 


## 2. boot stuck in purple screen after update

after update then boot stuck in purple screen

reason: intel update

solution: 

in grub, choose ubuntu, DONT PRESS ENETER ,presse 'e', add   

    dis_ucode_ldr  
    
after the line start with linux , then boot (press F10)

once boot, in termial : 

    sudo apt install intel-microcode=3.20180312.0~ubuntu18.04.1 

then hold package:  

    sudo apt-mark hold intel-microcode=3.20180312.0~ubuntu18.04.1

should solve the problem

## 3. cuda 10.1 install 

it sucks, as usual.

1. if not boot after install 

in grub, choose ubuntu ,presse 'e', add  

    modprobe.blacklist=nouveau  
    
after splash , the line starts with linux then boot,

if bootable, then step 2

2. blacklist nouveau

        sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

        sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

        cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

should see:

    blacklist nouveau

    options nouveau modeset=0

then 

    sudo update-initramfs -u

then install cuda driver, see step3

3. install cuda 



download deb file and install should work, if not, 

    sudo apt-get purge --remove cuda*

then install cuda driver, 

    sudo add-apt-repository ppa:graphics-drivers/ppa
    sudo apt update
    sudo apt install nvidia-driver-xxx (418 for cuda 10, 410 for cuda 9, ...)
    sudo reboot

once reboot

    nvidia-smi
    
see driver installed properly or not, if it does, then

Then download cuda run file and install cuda without install driver,

If it is still not working, well, a clean install ubuntu may help.

## 4. Turn off screen 

in terminal: 

    xset dpms force off

permanent: add

    alias screenoff='xset dpms force off' 
    
to .bashrc

then in the futurn, just type 'screenoff' in the terminal 

## 5. Slow wifi
resaon : 18.04 will use 802.11n but the router may use a/b/g

Check if it works first with:

    sudo modprobe -r iwlwifi

    sudo modprobe iwlwifi 11n_disable=1

Make it permanent with this command:

    echo "options iwlwifi 11n_disable=1" | sudo tee /etc/modprobe.d/iwlwifi.conf

### Declartion

It is just proplem I have with my zenbook, if it distroys your laptop, well, cest la vie

If you have other problems , I may be able to help (if the system is not fresh installed, 

then the possibitties will be way to high)

my mail: a36402191234@gmail.com



