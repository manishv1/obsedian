![[Raspberry_pi_Build_Process.xlsx]]

## Refer attached file --> Raspberry_pi_Build_Process.xlsx


https://hechao.li/2021/12/20/Boot-Raspberry-Pi-4-Using-uboot-and-Initramfs/

other than RPI other nice article  to check it later 
https://hechao.li/2020/06/09/Mini-Container-Series-Part-0-Not-a-Real-Container/

Refer section 4.4 Install u-boot   in above link


**NOTE:** Raspberry Pi has its own proprietary bootloader, which is loaded by the ROM code and is capable of loading the kernel. However, since I’d like to use the open source `u-boot`, I’ll need to configure the Raspberry Pi boot loader to load `u-boot` and then let `u-boot` load the kernel.


```
# Download Raspberry Pi firmware/boot directory
$ svn checkout https://github.com/raspberrypi/firmware/trunk/boot

# Copy Raspberry Pi 4 bootloader into boot partition
$ sudo cp boot/{bootcode.bin,start4.elf} /mnt/boot/

# Let Raspberry Pi 4 bootloader load u-boot
$ cat << EOF > config.txt
enable_uart=1
arm_64bit=1
kernel=u-boot.bin
EOF
$ sudo mv config.txt /mnt/boot/
```

![[Pasted image 20241007174916.png]]![[Pasted image 20241007183344.png]]![[Pasted image 20241007183428.png]]![[Pasted image 20241007183458.png]]![[Pasted image 20241007183709.png]]![[Pasted image 20241007183738.png]]![[Pasted image 20241008214626.png]]