

● Cross-compile value proposition 
● Preparing the system for cross-compiler installation 
● Cross-compiler installation steps 
● Demo – install arm and arm64 
● Compiling on architectures 
● Demo – compile arm and arm64 
● Automating cross-compile testing 
● Upstream cross-compile testing activity 
● References and Package repositories



Setting up cross-compile environment is the first and necessary step



Cross-compiler packages


● Ubuntu arm packages (12.10 or later)
	– gcc-arm-linux-gnueabi 
	– gcc-arm-linux-gnueabihf 
● Ubuntu arm64 packages (13.04 or later) 
	– use arm64 repo for older Ubuntu releases. 
	– gcc-4.7-aarch64-linux-gnu 
● Ubuntu keeps adding support for compilers. 
Search Ubuntu repository for packages.

![[VariousArchitectureted image 20241006105834.png]]Embedded Debian Project is a good resource for alpha, mips, mipsel, powerpc, sh, and sparc cross-compilers.
– gcc-4.7-alpha-linux-gnu 
– gcc-4.7-mips-linux-gnu 
– gcc-4.7-mipsel-linux-gnu 
– gcc-4.7-powerpc-linux-gnu 
– gcc-4.7-sh4-linux-gnu 
– gcc-4.7-sparc-linux-gnu



Fedora repo and Fedora Epel Repo are a good sources for several
cross-compilers and binutils rpms
– blackfin
● binutils-bfin-linux-gnu-2.23.51.0.3-1.fc17.x86_64.rpm
● gcc-bfin-linux-gnu-4.7.1-0.1.20120606.fc17.x86_64.rpm
– c6x
● binutils-c6x-linux-gnu-2.23.51.0.3-1.fc17.x86_64.rpm
● gcc-c6x-linux-gnu-4.7.2-2.aa.20121114svn.fc17.x86_64.rpm
– tile
● binutils-tile-linux-gnu-2.23.51.0.3-1.fc17.x86_64.rpm
● gcc-tile-linux-gnu-4.7.2-2.aa.20121114svn.fc17.x86_64.rpm



<span style="color:rgb(0, 176, 80)">Preparing the system for cross-compiler installation
</span>


<span style="color:rgb(0, 176, 80)">● Choose an x86-64 system </span>
<span style="color:rgb(0, 176, 80)">● Install Ubuntu 20.04 or later.</span> 



Install common packages

sudo apt-get install build-essential
sudo apt-get install binutils-multiarch
sudo apt-get install ncurses-dev
sudo apt-get install alien

<span style="color:rgb(192, 0, 0)">*Note: ncurses-dev is required to run menuconfig and alien to generate .deb from .rpm*</span><span style="color:rgb(192, 0, 0)"> </span> 


Install Cross Compilers :


alpha 
sudo apt-get install --install-recommends gcc-4.7-alpha-linux-gnu sudo ln -s /usr/bin/alpha-linux-gnu-gcc-4.7 /usr/bin/alpha-linux-gnu-gcc



arm 
sudo apt-get install gcc-arm-linux-gnueabi


arm64
sudo apt-get install --install-recommends gcc-4.7-aarch64-linux-gnu sudo ln -s /usr/bin/aarch64-linux-gnu-gcc-4.7 /usr/bin/aarch64-linux-gnu-gcc


mips
sudo apt-get install --install-recommends gcc-4.7-mips-linux-gnu sudo ln -s /usr/bin/mips-linux-gnu-gcc-4.7 /usr/bin/mips-linux-gnu-gcc


![[NativeCompilation.png]]

![[Cross-Compiling.png]]     


ARM architectures
To find out for which of these (32 bit or 64 bit ARM) you need to compile, the easiest is to look at the output of uname -m.

For x86_64 (standard PC):

~$ uname -m
x86_64


32 bit ARM:
uname -m
armv7l

64 bit ARM (or aarch64):
root@armv8:/ # uname -m
aarch64


# Prerequisites

Before we can start compiling, we need to install the necessary packages and tools for cross compiling for ARM. These include the standard tools needed for compiling native:

## **For 32 bit ARM** (arm):

jensd@deb10:~$ **sudo apt install gcc make gcc-arm-linux-gnueabi binutils-arm-linux-gnueabi**
Reading package lists... Done
Building dependency tree
...
Processing triggers for man-db (2.8.5-2) ...
Processing triggers for libc-bin (2.28-10) ...

## **For 64 bit ARM** (aarch64):

jensd@deb10:~$ **sudo apt install gcc make gcc-aarch64-linux-gnu binutils-aarch64-linux-gnu**
Reading package lists... Done
...
Processing triggers for man-db (2.8.5-2) ...
Processing triggers for libc-bin (2.28-10) ...