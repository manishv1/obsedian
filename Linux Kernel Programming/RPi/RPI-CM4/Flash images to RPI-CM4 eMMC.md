
![[Pasted image 20241017231445.png]]

https://www.jeffgeerling.com/blog/2020/how-flash-raspberry-pi-os-compute-module-4-emmc-usbboot

usbboot is name tool provided by RPI to flash  images on eMMC


The Raspberry Pi Compute Module 4 comes in two main flavors: one with built-in eMMC storage, and one without it. If you opt for a Compute Module 4 with built-in eMMC storage, and you want to write a new OS image to the Compute Module, or manually edit files on the boot volume, you can do that just the same as you would a microSD card—but you need to first make the eMMC storage mountable on another computer.

This blog post shows how to mount the eMMC storage on another computer (in my case a Mac, but the process is very similar on Linux), and then how to flash a new OS image to it.



Prerequisite to build rpiboot/usbboot :


First, make sure you have the `libusb` library installed:
```
sudo apt-get install pkg-config
sudo apt install libusb-1.0-0-dev
```


Second, clone the usbboot repository to your computer:
```
git clone --depth=1 https://github.com/raspberrypi/usbboot
```

cd usbboot
make 


Now there should be an `rpiboot` executable in the directory. To mount the eMMC storage, run:
```
sudo ./rpiboot
```
And a few seconds later, after it finishes doing its work, you should see the `boot` volume mounted on your Mac (or on whatever Linux computer you're using). You might also notice the D2 LED lighting up; that means there is disk read/write activity on the eMMC.

After running  "sudo ./rpiboot" on the host machine, do following 

1. Insure jumper is connected properly 
2. USB is connected 
3. Power on the board with 12V DC battery 
4. You will find two volumes are mounted on the Host Platform 





# How to update the Raspberry Pi Compute Module 4 Bootloader / EEPROM




https://www.jeffgeerling.com/blog/2022/how-update-raspberry-pi-compute-module-4-bootloader-eeprom

So far not able to find the source code  pieeprom-xx.xx.xx.bin file. 


## Enabling UART messages as well as shell prompt on Host machine 

### Add  "enable_uart=1" in /boot/config.txt file 


Not sure what is this for --:
dtoverlay=dwc2,dr_mode=host


--> Attach Keyboard and Mouse on the rpi-cm4  i/o board 
--> Ensure USB type B cable is not connected to I/O board , otherwise keyboard mouse did not worked 


Use some small commands like -->

lsusb --> to check for the USB devices 
**journalctl:** This is the journal management tool that allows you to view, search, and filter system logs.
The command `journalctl -b | grep usb` is used to filter the system journal for entries related to USB devices

## Build u-boot for RPI-CM4 

https://forums.raspberrypi.com/viewtopic.php?t=314845
https://stackoverflow.com/questions/68282482/boot-process-for-raspberry-pi-compute-module-4-running-yocto-image-with-u-boot

https://andrei.gherzan.ro/linux/uboot-on-rpi/    (Good )

https://www.raspberrypi.com/documentation/computers/config_txt.html
https://jamesachambers.com/full-compute-module-4-raspberry-pi-setup-imaging-guide/

https://pip.raspberrypi.com/categories/685-whitepapers-app-notes/documents/RP-003466-WP/Boot-Security-Howto.pdf
## Build Kernel for RPI-CM4 


![[Raspberry_pi_Build_Process.xlsx]]