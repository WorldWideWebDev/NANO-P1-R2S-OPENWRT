# NANO-P1-R2S-OPENWRT

Introduction

Overview

Front

Back

Case
The NanoPi R2S(as "R2S") is an open source platform with dual-Gbps Ethernet ports designed and developed by FriendlyElec for IoT applications.
The NanoPi R2S uses the RK3328 SoC. It has two Gbps Ethernet ports and 1G DDR4 RAM. FriendlyElec ported an OpenWrt system for it. It works with Docker CE. It is a good platform for developing IoT applications, NAS applications, smart home gateways etc.
2 Hardware Spec
CPU: Rockchip RK3328, Quad-core Cortex-A53
DDR4 RAM: 1GB
Network：
Internal 10/100/1000M Ethernet Port x 1
USB3.0 converted 10/100/1000M Ethernet Port x 1
USB2.0 Host: Type-A x1
MicroSD Slot x 1
MicroUSB: power input and USB Slave
Debug Serial Port: 3.3V TTL, 3-pin 2.54mm pitch connector, 1500000 bauds
LED: LED x 3
KEY: KEY x 1 programmable
PC Size: 55.6 x 52mm
Power Supply: DC 5V/2A
Temperature measuring range: 0℃ to 80℃
OS/Software: U-boot，Ubuntu-Core，OpenWrt 


Network Transmission Rates
TX	RX
WAN	941 Mbps	941 Mbps
LAN	941 Mbps	941 Mbps
Notes：	1. test utility: iperf 
2. use indepedent IP address section and test with a PC in simplex communication mode
3 Diagram, Layout and Dimension
3.1 Layout
NanoPi R2S Layout

GPIO 24-Pin Spec
Pin#	Name	Linux gpio	Pin#	Name	Linux gpio
1	SYS_3.3V		2	VDD_5V	
3	I2C0_SDA / GPIO2_D1	89	4	VDD_5V	
5	I2C0_SCL / GPIO2_D0	88	6	GND	
7	GPIO2_A2 / IR_RX	66	8	UART1_TX / GPIO3_A4	100
9	GND		10	UART1_RX / GPIOG3_A6	102
For more details refer to:NanoPi_R2S_V1.0_1912-Schematic.pdf
Dimensional Diagram：NanoPi R2S PCB file in dxf format
4 Get Started
4.1 Essentials You Need
Before starting to use your NanoPi R2S get the following items ready

NanoPi R2S
MicroSD Card/TF Card: Class 10 or Above, minimum 8GB SDHC
MicroUSB 5V/2A power adapter
A host computer running Ubuntu 16.04 64-bit system
4.2 TF Cards We Tested
To make your NanoPi R2S boot and run fast we highly recommend you use a Class10 8GB SDHC TF card or a better one. The following cards are what we used in all our test cases presented here:

SanDisk TF 8G Class10 Micro/SD High Speed TF card:
SanDisk microSD 8G



4.3 Install OS
4.3.1 Download Image Files
Go to download link to download the image files under the officail-ROMs directory and the flashing utility under the tools directory:

OPEN WRT WEBSITE 
https://openwrt.org/toh/friendlyarm/friendlyarm_nanopi_r2s  


Image Files:
http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-ext4-sysupgrade.img.gz

Upgrade File:
http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-squashfs-sysupgrade.img.gz

Flashing Utility:
win32diskimager.rar	Windows utility. Under Linux users can use "dd"
BALENA ETCHER
https://www.balena.io




5.2 First boot
For the first boot, the system needs to do the following initialization work： 
1）Extended root file system 
2）Initial setup（will execute /root/setup.sh) 
So you need to wait for a while (about 2~3 minutes) to boot up for the first time, and then set FriendlyWrt, you can enter the ttyd terminal on the openwrt webpage, when the prompt is displayed as root@FriendlyWrt, it means the system has been initialized.

root@YOUR-DEVICE-IP
root@openwrt
root@192.168.1.1
5.3 Account & Password
The default password is password (empty password in some versions). Please set or change a safer password for web login and ssh login. It is recommended to complete this setting before connecting NanoPi-R2S to the Internet.

 Before you can log into the web interface you must install the luci web portal
 
 ssh into your device and update it:-
 opkg update
 opkg install luci
 
 Now you may open a webpage and log into 192.168.1.1
 password is not required but you shoul update a password as prompted
 
5.4 Network Connection
Use a network cable to connect NanoPi-R2S's WAN to a master router and the board will get an IP address via DHCP. Login into the router and check NanoPi-R2S's IP address.

5.5 Login OpenWrt
Connect the PC to the LAN port of NanoPi-R2S. If your PC without a built-in ethernet port, connect the LAN port of the wireless AP to the LAN port of NanoPi-R2S, and then connect your PC to the wireless AP via WiFi , Enter the following URL on your PC's browser to access the admin page: 


http://192.168.1.1/
http://[fd00:ab:cd::1]
The above is the LAN port address of NanoPi-R2S. The IP address of the WAN port will be dynamically obtained from your main router through DHCP.

5.6 Recommended security settings
The following settings are highly recommended to complete before connecting NanoPi-R2S to the Internet。

Set a secure password
Only allow access to ssh from lan, change the port
Only allow local devices to access luci
Edit /etc/config/uhttpd，Change the original 0.0.0.0 and [::] addresses to the local lan address, for example：

	# HTTP listen addresses, multiple allowed
	list listen_http	192.168.1.1:80
	list listen_http	[fd00:ab:cd::1]:80
 
	# HTTPS listen addresses, multiple allowed
	list listen_https	192.168.1.1:443
	list listen_https	[fd00:ab:cd::1]:443
Restart the service:

/etc/init.d/uhttpd restart
5.7 Safe shutdown operation
Enter the ttyd terminal, enter the poweroff command and hit enter, wait until the led light is off, and then unplug the power supply.

5.8 Install Software Packages
5.8.1 Update Package List
Before install software packages update the package list:

$ opkg update
5.8.2 List Available Packages
$ opkg list
5.8.3 List Installed Packages
$ opkg list-installed
5.8.4 Install Packages
$ opkg install <package names>
5.8.5 Remove Packages
$ opkg remove <package names>
5.9 Disable IPv6
sed -i -e "s/DISABLE_IPV6=0/DISABLE_IPV6=1/g" /root/setup.sh
rm -f /etc/board.json /etc/config/system /etc/config/network /etc/config/wireless /etc/firstboot_*
reboot

5.11 Let openWrt regenerate network settings
This method will trigger FriendlyWrt to re-identify the hardware model and generate the network configuration under /etc/config, which is similar but not completely equivalent to restoring factory settings:

rm -f /etc/board.json /etc/config/system /etc/config/network /etc/config/wireless /etc/firstboot_*
reboot
The /root/setup.sh initialization script will be executed again at the next boot, so you can debug the /root/setup.sh script through this method.

5.12 Use USB2LCD to view IP and temperature
Plug the USB2LCD module to the USB interface ofNanoPi-R2S and power on, the IP address and CPU temperature will be displayed on the LCD:
R2S-usb2lcd-01.jpg

5.13 Work with USB WiFi Device
5.13.1 Check USB WiFi Device with Command Line Utility
(1) Click on "services>ttyd" to start the command line utility
R2s-wrt-jellyfin-002.jpg
(2) Make sure no USB devices are connected to your board and run the following command to check if any USB devices are connected or not

lsusb
R2swrt+usbwifi-09.jpg

(3) Connect a USB WiFi device to the board and run the command again

lsusb
You will see a new device is detected. In our test the device's ID was 0BDA:C811
R2swrt+usbwifi-10.jpg

(4) Type your device's ID (in our case it was "0BDA:C811" or "VID_0BDA&PID_C811") in a search engine and you may find a device that matches the ID. In our case the device we got was Realtek 8811CU.

5.13.2 Configure a USB WiFi Device as AP
(1) Connect a USB WiFi device to the NanoPi-R2S. We recommend you to use the following devices:
R2swrt+usbwifi-08.jpg
Note: devices that match these VID&PIDs would most likely work.
(2) Click on "System>Reboot" and reboot your NanoPi-R2S
R2swrt+usbwifi-01.jpg

R2swrt+usbwifi-02.jpg
(3) Click on "Network>Wireless" to enter the WiFi configuration page
R2swrt+usbwifi-03.jpg
(4) Click on "Edit" to edit the configuration
R2swrt+usbwifi-04.jpg
(5) On the "Interface Configuration" page you can set the WiFi mode and SSID, and then go to "Wireless Security" to change the password. By default the password is "password". After you make your changes click on "Save" to save
R2swrt+usbwifi-05.jpg

R2swrt+usbwifi-06.jpg
(6) After you change the settings you can use a smartphone or PC to search for WiFi
R2swrt+usbwifi-07.png

5.14 Work with Docker Applications
5.14.1 Work with Docker: Install JellyBin
mkdir -p /jellyfin/config
mkdir -p /jellyfin/videos
docker run --restart=always -d -p 8096:8096 -v /jellyfin/config:/config -v /jellyfin/videos:/videos jellyfin/jellyfin:10.1.0-arm64 -name myjellyfin
After installation, visit port 8096 and here is what you would find:
FriendlyWrt+JerryFin

5.14.2 Work with Docker: Install Personal Nextcloud
mkdir /nextcloud -p
docker run -d -p 8888:80  --name nextcloud  -v /nextcloud/:/var/www/html/ --restart=always --privileged=true  arm64v8/nextcloud
After installtion, visit port 8888.



5.15 Mount smbfs
mount -t cifs //192.168.1.10/shared /movie -o username=xxx,password=yyy,file_mode=0644
5.16 Use sdk to compile the package
5.16.1 Install the compilation environment
Download and run the following script on 64-bit Ubuntu (version 18.04+): How to setup the Compiling Environment on Ubuntu bionic

5.16.2 Download and decompress sdk from the network disk
The sdk is located in the toolchain directory of the network disk:

tar xvf ~/dvd/RK3328/toolchain/friendlywrt-kernel-5.x.y/openwrt-sdk-19.07.5-rockchip-rk3328_gcc-7.5.0_musl.Linux-x86_64.tar.xz
# If the path is too long, it will cause some package compilation errors, so change the directory name here
mv openwrt-sdk-19.07.5-rockchip-rk3328_gcc-7.5.0_musl.Linux-x86_64 sdk
cd sdk
./scripts/feeds update -a
./scripts/feeds install -a
5.16.3 Compile the package
download the source code of the example (a total of 3 examples are example1, example2, example3), and copy to the package directory:

git clone https://github.com/mwarning/openwrt-examples.git
cp -rf openwrt-examples/example* package/
rm -rf openwrt-examples/
Then enter the configuration menu through the following command:

make menuconfig
In the menu, select the following packages we want to compile (actually selected by default):

"Utilities" => "example1"
"Utilities" => "example3"
"Network" => "VPN" => "example2"
execute the following commands to compile the three software packages:

make package/example1/compile V=99
make package/example2/compile V=99
make package/example3/compile V=99
After the compilation is successful, you can find the ipk file in the bin directory, as shown below:

$ find ./bin -name example*.ipk
./bin/packages/aarch64_cortex-a53/base/example2_0.1-1_aarch64_cortex-a53.ipk
./bin/packages/aarch64_cortex-a53/base/example3_0.1-1_aarch64_cortex-a53.ipk
./bin/packages/aarch64_cortex-a53/base/example1_0.1-1_aarch64_cortex-a53.ipk
5.16.4 Install the ipk to NanoPi
You can use the scp command to upload the ipk file to NanoPi:

cd ./bin/packages/aarch64_cortex-a53/base/
scp example*.ipk root@192.168.2.1:/root/
Then use the opkg command to install them:

cd /root/
opkg install example2_0.1-1_aarch64_cortex-a53.ipk
opkg install example3_0.1-1_aarch64_cortex-a53.ipk
opkg install example1_0.1-1_aarch64_cortex-a53.ipk
5.17 Compile u-boot,kernel or friendlywrt
Refer to：
sd-fuse_rk3328
How to Build FriendlyWrt
5.18 Serial port debug
Connect to NanoPi R2S with screen :

screen /dev/ttyUSB0 1500000 8N1
6 Compile FriendlyWrt
6.1 Download Code
mkdir friendlywrt-rk3328
cd friendlywrt-rk3328
repo init -u https://github.com/friendlyarm/friendlywrt_manifests -b master-v19.07.1 -m rk3328.xml --repo-url=https://github.com/friendlyarm/repo  --no-clone-bundle
repo sync -c  --no-clone-bundle
6.2 1-key Compile
./build.sh nanopi_r2.mk
All the components (including u-boot, kernel, and friendlywrt) are compiled and the sd card image will be generated.

7 Work with FriendlyCore
7.1 FriendlyCore User Account
Non-root User:
   User Name: pi
   Password: pi
Root:
   User Name: Root
   Password: fa
7.2 Update Software Packages
$ sudo apt-get update
7.3 Setup Network Configurations
By default "eth0" is assigned an IP address obtained via dhcp. If you want to change the setting you need to change the following file:

vi /etc/network/interfaces.d/eth0
For example if you want to assign a static IP to it you can run the following commands:

auto eth0
iface eth0 inet static
    address 192.168.1.231
    netmask 255.255.255.0
    gateway 192.168.1.1
To change the setting of "eth1" you can add a new file similar to eth0's configuration file under the /etc/network/interfaces.d/ directory.

8 Make Your Own OS Image
Please refre this link：
sd-fuse_rk3328
9 Resources
9.1 Datasheets and Schematics
Schematics
NanoPi_R2S_V1.0_1912-Schematic.pdf
PCB Dimensional Diagram
NanoPi_R2S_V1.0_1912 PCB file in dxf format
Datasheet
RK3328 Datasheet Rockchip_RK3328_Datasheet.pdf
10 Update Logs
10.1 2021-01-15
upgraded kernel to 5.10
upgraded friendlywrt to 19.07.5
10.2 2020-09-04
Improved the stability of the usb3.0 driver (integrated with official Rockchip updates) and the stability of the r8153 ethernet card
Update the driver of the 8153 ethernet card, using mainline kernel's version, the version is v1.11.11
Enable serial port UART1 for R2S and NEO3
10.3 2020-08-31
Kernel updated to 5.4.61
Use the driver r8152-2.13.0 downloaded from Realtek official website (https://www.realtek.com/en/downloads)
Improve the USB network card driver, WLAN<->LAN bridge speed can reach 700/500 Mbits/sec
Add rockhip rng (hardware random number) driver
10.4 2020-07-10
10.4.1 FriendlyWrt
upgraded kernel to 5.4.50
added support for NanoPi-NEO3
10.5 2020-05-14
10.5.1 FriendlyWrt
upgraded kernel to 5.4.40，fixed bpfilter issue
added new usb wifi（rtl8812au）
fixed overlayfs issue
10.6 2020-02-25
10.6.1 FriendlyWrt
10.6.1.1 update to v19.07.1，please use branch master-v19.07.1：
mkdir friendlywrt-rk3328
cd friendlywrt-rk3328
repo init -u https://github.com/friendlyarm/friendlywrt_manifests -b master-v19.07.1 -m rk3328.xml --repo-url=https://github.com/friendlyarm/repo --no-clone-bundle
repo sync -c --no-clone-bundle
10.6.1.2 fixed some issues：
fixed bpfilter module issue
updated feeds to the latest commit 
removed modemmanager and mwan3 plugins
adjusted cpu scaling governor, optimized startup speed
10.7 2020-02-20
10.7.1 FriendlyWrt
Optimized openssl performance
Added support for PWM fan, support fan speed control (platform: rk33xx)
10.8 2020-01-18
Initial Release

Rockchip Schematics
http://wiki.friendlyarm.com/wiki/images/5/59/SCH_NanoPi_R2S_V1.0-1912.pdf



OPEN WRT SITE

 Table of Contents 
FriendlyARM NanoPi R2S
Getting started with a new Device Page
Keep the articles modular
Supported Versions
Experimental Versions
Hardware Highlights
Installation
Flash Layout
OEM easy installation
OEM installation using the TFTP method
Upgrading OpenWrt
LuCI Web Upgrade Process
Terminal Upgrade Process
Debricking
Failsafe mode
Basic configuration
Specific Configuration
Network interfaces
Switch Ports (for VLANs)
Buttons
Hardware
Info
Photos
Opening the case
Serial
JTAG
Bootloader mods
Hardware mods
Bootlogs
OEM bootlog
OpenWrt bootlog
Notes
Tags
FriendlyARM NanoPi R2S
Under Construction!
This page is currently under construction. You can edit the article to help completing it.

The NanoPi R2S uses the RK3328 SoC. It has two Gbps Ethernet ports and 1G DDR4 RAM. Good cheap for gateway router.

Support Forums https://forum.openwrt.org/t/nanopi-r2s-is-a-great-openwrt-device/65374

Others git

https://github.com/jayanta525/openwrt-nanopi-r2s
https://github.com/klever1988/nanopi-openwrt
https://github.com/QiuSimons/R2S-OpenWrt
nanopi-r2s

FIXME

Getting started with a new Device Page

This is an empty template that suggests the information that should be present on a well-constructed Device Page. This means, that you have to fill it with life and information.
There are several “fixme” tags with text on a light background (like this text) throughout this template. As you fill in the page, remove those tags so that people can judge its completeness.
When there are no more “fixme” tags left, delete this one too, along with the <WRAP> that encloses it.
Keep the articles modular

Please include only model specific information, omit bla,bla and put everything generic into separate articles
If you have no time to write certain stuff, link to docs
base-system should lead the way, do not explain this again
DO NOT provide a complete howto here! Instead groom the general documentation.
Supported Versions

Brand	Model	Version	Current Release	OEM Info	Forum Search	Technical Data
FriendlyARM	NanoPi R2S		snapshot	http://wiki.friendlyarm.com/wiki/index.php/NanoPi_R2S	NanoPi R2S	View/Edit data
Unsupported Functions
Experimental Versions

None at this time.

Hardware Highlights

Model	Version	SoC	CPU MHz	CPU Cores	Flash MB	RAM MB	WLAN Hardware	WLAN2.4	WLAN5.0	100M ports	Gbit ports	Modem	USB
NanoPi R2S		Rockchip RK3328	1300	4	microSDHC	1024	none	-	-	-	2	-	1x 2.0
Installation

Model	Version	Current Release	Firmware OpenWrt snapshot Install	Firmware OpenWrt snapshot Upgrade	Firmware OEM Stock
NanoPi R2S		snapshot	http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-ext4-sysupgrade.img.gz	http://downloads.openwrt.org/snapshots/targets/rockchip/armv8/openwrt-rockchip-armv8-friendlyarm_nanopi-r2s-squashfs-sysupgrade.img.gz	http://download.friendlyarm.com/nanopir2s
Uncompress the OpenWrt …sysupgrade.img.gz image and write it to a micro SD card using dd.

Flash Layout

FIXME Find out flash layout, then add the flash layout table here (copy, paste, modify the example).

Please check out the article Flash layout. It contains examples and explanations that describe how to document the flash layout.

OEM easy installation

FIXME The instructions below are for Broadcom devices and only serve as an example.
Remove / modify them if they do not apply to this particular device!

This section deals with

How you install OpenWrt from a device freshly opened
The steps required such as reset to factory defaults if the device has already been configured
Note: Reset router to factory defaults if it has been previously configured.

Browse to http://192.168.1.1/Upgrade.asp
Upload .bin file to router
Wait for it to reboot
Telnet to 192.168.1.1 and set a root password, or browse to http://192.168.1.1 if LuCI is installed.
OEM installation using the TFTP method

→ generic.flashing.tftp

Specific values needed for tftp

FIXME Enter values for “FILL-IN” below

Bootloader tftp server IPv4 address	FILL-IN
Bootloader MAC address (special)	FILL-IN
Firmware tftp image	Latest OpenWrt release (NOTE: Name must contain “tftp”)
TFTP transfer window	FILL-IN seconds
TFTP window start	approximately FILL-IN seconds after power on
TFTP client required IP address	FILL-IN
Upgrading OpenWrt

→ generic.sysupgrade

FIXME These are generic instructions. Update with your router's specifics.

LuCI Web Upgrade Process

Browse to http://192.168.1.1/cgi-bin/luci/mini/system/upgrade/ LuCI Upgrade URL
Upload image file for sysupgrade to LuCI
Wait for reboot
Terminal Upgrade Process

If you don't have a GUI (LuCI) available, you can alternatively upgrade via the command line. There are two command line methods for upgrading:

sysupgrade
mtd
Note: It is important that you put the firmware image into the ramdisk (/tmp) before you start flashing.

sysupgrade

Login as root via SSH on 192.168.1.1, then enter the following commands:
cd /tmp
wget http://downloads.openwrt.org/snapshots/trunk/XXX/xxx.abc
sysupgrade /tmp/xxx.abc
mtd

If sysupgrade does not support this router, use mtd.

Login as root via SSH on 192.168.1.1, then enter the following commands:
cd /tmp
wget http://downloads.openwrt.org/snapshots/trunk/XXX/xxx.abc
mtd write /tmp/xxx.abc linux && reboot
Debricking

→ generic.debrick

Failsafe mode

→ failsafe_and_factory_reset

Basic configuration

→ Basic configuration After flashing, proceed with this.
Set up your Internet connection, configure wireless, configure USB port, etc.

Specific Configuration

FIXME Please fill in real values for this device, then remove the EXAMPLEs

Network interfaces

The default network configuration is:

Interface Name	Description	Default configuration
br-lan	EXAMPLE LAN & WiFi	EXAMPLE 192.168.1.1/24
vlan0 (eth0.0)	EXAMPLE LAN ports (1 to 4)	EXAMPLE None
vlan1 (eth0.1)	EXAMPLE WAN port	EXAMPLE DHCP
wl0	EXAMPLE WiFi	EXAMPLE Disabled
Switch Ports (for VLANs)

FIXME Please fill in real values for this device, then remove the EXAMPLEs

Numbers 0-3 are Ports 1-4 as labeled on the unit, number 4 is the Internet (WAN) on the unit, 5 is the internal connection to the router itself. Don't be fooled: Port 1 on the unit is number 3 when configuring VLANs. vlan0 = eth0.0, vlan1 = eth0.1 and so on.

Port	Switch port
Internet (WAN)	EXAMPLE 4
LAN 1	EXAMPLE 3
LAN 2	EXAMPLE 2
LAN 3	EXAMPLE 1
LAN 4	EXAMPLE 0
Buttons

→ hardware.button on howto use and configure the hardware button(s). Here, we merely name the buttons, so we can use them in the above Howto.

FIXME Please fill in real values for this device, then remove the EXAMPLEs

The FriendlyARM NanoPi R2S has the following buttons:

BUTTON	Event
EXAMPLE Reset	reset
EXAMPLE Secure Easy Setup	ses
EXAMPLE No buttons at all.	-
Hardware

Info

FIXME

This table is automatically generated, once the correct filters for Brand and Model are set.
If you see “Nothing.” instead of a table, please edit this section and adjust the filters with the proper Brand and Model. Just try, it's easy.
If you still don't see a table here, or a table filled with '¿': Is there already a Techdata page available for FriendlyARM NanoPi R2S ? If not: Create one.
If you see a table with the desired device data, everything is OK and you can delete this text and the <WRAP> that encloses it.
If it still doesn't work: Don't panic, calm down, take a deep breath and contact a wiki admin (tmomas) for help.
Nothing.
Photos

Front:
Insert photo of front of the casing

Back:
Insert photo of back of the casing

Backside label:
Insert photo of backside label

Opening the case

Note: This will void your warranty!

FIXME Describe what needs to be done to open the device, e.g. remove rubber feet, adhesive labels, screws, …

To remove the cover and open the device, do a/b/c
Main PCB:
Insert photo of PCB

Serial

→ port.serial general information about the serial port, serial port cable, etc.

How to connect to the Serial Port of this specific device:
Insert photo of PCB with markings for serial port

Instructions

For debug purposes (*nix version):

Connect USB-TTL/UART adapter to USB PC
Connect TX/RX/GND pins of adapter to Debug UART pins on NanoPi R2S board (3 pins near microUSB)
Locate USB device name for USB-TTL/UART adapter in /dev
Run screen /dev/ttyUSB0 1500000 8N1
Power on NanoPi R2S (plug in 5V)
Note: Debug UART port is 3.3V TTL. Please, be aware that adapter will support 3.3V

FIXME Replace EXAMPLE by real values.

Serial connection parameters
for FriendlyARM NanoPi R2S @@Version@@	1500000 8N1
JTAG

→ port.jtag general information about the JTAG port, JTAG cable, etc.

How to connect to the JTAG Port of this specific device:
Insert photo of PCB with markings for JTAG port

Bootloader mods

→ bootloader

Hardware mods

None so far.

Bootlogs

OEM bootlog



COPY HERE THE BOOTLOG WITH THE ORIGINAL FIRMWARE




OpenWrt bootlog



[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x410fd034]
[    0.000000] Linux version 5.4.72 (builder@buildhost) (gcc version 8.4.0 (OpenWrt GCC 8.4.0 r14798-e91344776b)) #0 SMP PREEMPT Thu Oct 29 13:35:03 2020
[    0.000000] Machine model: FriendlyElec NanoPi R2S
[    0.000000] earlycon: uart8250 at MMIO32 0x00000000ff130000 (options '')
[    0.000000] printk: bootconsole [uart8250] enabled
[    0.000000] cma: Reserved 8 MiB at 0x000000003f800000
[    0.000000] On node 0 totalpages: 261632
[    0.000000]   DMA32 zone: 4088 pages used for memmap
[    0.000000]   DMA32 zone: 0 pages reserved
[    0.000000]   DMA32 zone: 261632 pages, LIFO batch:63
[    0.000000] psci: probing for conduit method from DT.
[    0.000000] psci: PSCIv1.1 detected in firmware.
[    0.000000] psci: Using standard PSCI v0.2 function IDs
[    0.000000] psci: MIGRATE_INFO_TYPE not supported.
[    0.000000] psci: SMC Calling Convention v1.0
[    0.000000] percpu: Embedded 21 pages/cpu s45912 r8192 d31912 u86016
[    0.000000] pcpu-alloc: s45912 r8192 d31912 u86016 alloc=21*4096
[    0.000000] pcpu-alloc: [0] 0 [0] 1 [0] 2 [0] 3
[    0.000000] Detected VIPT I-cache on CPU0
[    0.000000] CPU features: detected: ARM erratum 845719
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 257544
[    0.000000] Kernel command line: console=ttyS2,1500000 earlycon=uart8250,mmio32,0xff130000 root=PARTUUID=5452574f-02 rw rootwait
[    0.000000] Dentry cache hash table entries: 131072 (order: 8, 1048576 bytes, linear)
[    0.000000] Inode-cache hash table entries: 65536 (order: 7, 524288 bytes, linear)
[    0.000000] mem auto-init: stack:off, heap alloc:off, heap free:off
[    0.000000] Memory: 1004768K/1046528K available (7614K kernel code, 572K rwdata, 2372K rodata, 1664K init, 715K bss, 33568K reserved, 8192K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
[    0.000000] rcu: Preemptible hierarchical RCU implementation.
[    0.000000] rcu:     RCU event tracing is enabled.
[    0.000000] rcu:     RCU restricting CPUs from NR_CPUS=256 to nr_cpu_ids=4.
[    0.000000]  Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=4
[    0.000000] NR_IRQS: 64, nr_irqs: 64, preallocated irqs: 0
[    0.000000] GIC: Using split EOI/Deactivate mode
[    0.000000] random: get_random_bytes called from start_kernel+0x29c/0x39c with crng_init=0
[    0.000000] arch_timer: cp15 timer(s) running at 24.00MHz (phys).
[    0.000000] clocksource: arch_sys_counter: mask: 0xffffffffffffff max_cycles: 0x588fe9dc0, max_idle_ns: 440795202592 ns
[    0.000008] sched_clock: 56 bits at 24MHz, resolution 41ns, wraps every 4398046511097ns
[    0.001333] Console: colour dummy device 80x25
[    0.001860] Calibrating delay loop (skipped), value calculated using timer frequency.. 48.00 BogoMIPS (lpj=96000)
[    0.002791] pid_max: default: 32768 minimum: 301
[    0.003449] Mount-cache hash table entries: 2048 (order: 2, 16384 bytes, linear)
[    0.004129] Mountpoint-cache hash table entries: 2048 (order: 2, 16384 bytes, linear)
[    0.006953] ASID allocator initialised with 32768 entries
[    0.007569] rcu: Hierarchical SRCU implementation.
[    0.009136] smp: Bringing up secondary CPUs ...
[    0.010321] Detected VIPT I-cache on CPU1
[    0.010408] CPU1: Booted secondary processor 0x0000000001 [0x410fd034]
[    0.011170] Detected VIPT I-cache on CPU2
[    0.011229] CPU2: Booted secondary processor 0x0000000002 [0x410fd034]
[    0.011934] Detected VIPT I-cache on CPU3
[    0.011988] CPU3: Booted secondary processor 0x0000000003 [0x410fd034]
[    0.012096] smp: Brought up 1 node, 4 CPUs
[    0.015396] SMP: Total of 4 processors activated.
[    0.015830] CPU features: detected: 32-bit EL0 Support
[    0.016298] CPU features: detected: CRC32 instructions
[    0.025157] CPU: All CPU(s) started at EL2
[    0.025564] alternatives: patching kernel code
[    0.038122] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    0.039072] futex hash table entries: 1024 (order: 4, 65536 bytes, linear)
[    0.040342] pinctrl core: initialized pinctrl subsystem
[    0.042001] NET: Registered protocol family 16
[    0.044763] DMA: preallocated 256 KiB pool for atomic allocations
[    0.046396] cpuidle: using governor menu
[    0.047165] Serial: AMBA PL011 UART driver
[    0.074017] HugeTLB registered 1.00 GiB page size, pre-allocated 0 pages
[    0.074638] HugeTLB registered 32.0 MiB page size, pre-allocated 0 pages
[    0.075245] HugeTLB registered 2.00 MiB page size, pre-allocated 0 pages
[    0.075872] HugeTLB registered 64.0 KiB page size, pre-allocated 0 pages
[    0.079816] sdmmc-regulator GPIO handle specifies active low - ignored
[    0.081808] iommu: Default domain type: Translated
[    0.083450] SCSI subsystem initialized
[    0.084071] usbcore: registered new interface driver usbfs
[    0.084621] usbcore: registered new interface driver hub
[    0.085214] usbcore: registered new device driver usb
[    0.086005] pps_core: LinuxPPS API ver. 1 registered
[    0.086456] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.087340] PTP clock support registered
[    0.088304] workqueue: max_active 576 requested for napi_workq is out of range, clamping between 1 and 512
[    0.090329] clocksource: Switched to clocksource arch_sys_counter
[    0.091193] VFS: Disk quotas dquot_6.6.0
[    0.091635] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
[    0.097636] thermal_sys: Registered thermal governor 'step_wise'
[    0.097644] thermal_sys: Registered thermal governor 'power_allocator'
[    0.098869] NET: Registered protocol family 2
[    0.100757] tcp_listen_portaddr_hash hash table entries: 512 (order: 1, 8192 bytes, linear)
[    0.101549] TCP established hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    0.102414] TCP bind hash table entries: 8192 (order: 5, 131072 bytes, linear)
[    0.103256] TCP: Hash tables configured (established 8192 bind 8192)
[    0.104033] UDP hash table entries: 512 (order: 2, 16384 bytes, linear)
[    0.104675] UDP-Lite hash table entries: 512 (order: 2, 16384 bytes, linear)
[    0.105767] NET: Registered protocol family 1
[    0.106205] PCI: CLS 0 bytes, default 64
[    0.108163] workingset: timestamp_bits=46 max_order=18 bucket_order=0
[    0.118679] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.119221] jffs2: version 2.2 (NAND) (SUMMARY) (ZLIB) (LZMA) (RTIME) (CMODE_PRIORITY) (c) 2001-2006 Red Hat, Inc.
[    0.122198] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 250)
[    0.123258] io scheduler mq-deadline registered
[    0.129915] dma-pl330 ff1f0000.dmac: Loaded driver for PL330 DMAC-241330
[    0.130614] dma-pl330 ff1f0000.dmac:         DBUFF-128x8bytes Num_Chans-8 Num_Peri-20 Num_Events-16
[    0.133896] Serial: 8250/16550 driver, 16 ports, IRQ sharing enabled
[    0.138644] printk: console [ttyS2] disabled
[    0.139166] ff130000.serial: ttyS2 at MMIO 0xff130000 (irq = 12, base_baud = 1500000) is a 16550A
[    0.140052] printk: console [ttyS2] enabled
[    0.140805] printk: bootconsole [uart8250] disabled
[    0.150815] loop: module loaded
[    0.151118] mtip32xx Version 1.3.1
[    0.151580] Loading iSCSI transport class v2.0-870.
[    0.154712] libphy: Fixed MDIO Bus: probed
[    0.156240] clk: failed to reparent clk_mac2io to gmac_clk: -22
[    0.156785] clk: failed to reparent clk_mac2io_ext to gmac_clk: -22
[    0.157396] rk_gmac-dwmac ff540000.ethernet: IRQ eth_wake_irq not found
[    0.157984] rk_gmac-dwmac ff540000.ethernet: IRQ eth_lpi not found
[    0.158734] rk_gmac-dwmac ff540000.ethernet: PTP uses main clock
[    0.159302] rk_gmac-dwmac ff540000.ethernet: phy regulator is not available yet, deferred probing
[    0.161524] dwc3 ff600000.dwc3: Failed to get clk 'ref': -2
[    0.162574] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    0.163155] ehci-pci: EHCI PCI platform driver
[    0.163622] ehci-platform: EHCI generic platform driver
[    0.166460] ehci-platform ff5c0000.usb: EHCI Host Controller
[    0.166991] ehci-platform ff5c0000.usb: new USB bus registered, assigned bus number 1
[    0.167823] ehci-platform ff5c0000.usb: irq 28, io mem 0xff5c0000
[    0.182365] ehci-platform ff5c0000.usb: USB 2.0 started, EHCI 1.00
[    0.183583] hub 1-0:1.0: USB hub found
[    0.183957] hub 1-0:1.0: 1 port detected
[    0.184824] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    0.185413] ohci-platform: OHCI generic platform driver
[    0.186231] ohci-platform ff5d0000.usb: Generic Platform OHCI controller
[    0.186905] ohci-platform ff5d0000.usb: new USB bus registered, assigned bus number 2
[    0.187743] ohci-platform ff5d0000.usb: irq 29, io mem 0xff5d0000
[    0.251002] hub 2-0:1.0: USB hub found
[    0.251377] hub 2-0:1.0: 1 port detected
[    0.252760] xhci-hcd xhci-hcd.0.auto: xHCI Host Controller
[    0.253274] xhci-hcd xhci-hcd.0.auto: new USB bus registered, assigned bus number 3
[    0.254153] xhci-hcd xhci-hcd.0.auto: hcc params 0x0220fe64 hci version 0x110 quirks 0x0000000002010010
[    0.255112] xhci-hcd xhci-hcd.0.auto: irq 163, io mem 0xff600000
[    0.256554] hub 3-0:1.0: USB hub found
[    0.256925] hub 3-0:1.0: 1 port detected
[    0.257681] xhci-hcd xhci-hcd.0.auto: xHCI Host Controller
[    0.258187] xhci-hcd xhci-hcd.0.auto: new USB bus registered, assigned bus number 4
[    0.258925] xhci-hcd xhci-hcd.0.auto: Host supports USB 3.0 SuperSpeed
[    0.259577] usb usb4: We don't know the algorithms for LPM for this host, disabling LPM.
[    0.260815] hub 4-0:1.0: USB hub found
[    0.261185] hub 4-0:1.0: 1 port detected
[    0.262132] usbcore: registered new interface driver usb-storage
[    0.262890] i2c /dev entries driver
[    0.265273] rk808 1-0018: chip id: 0x8050
[    0.271440] rk808-regulator rk808-regulator: there is no dvs0 gpio
[    0.272025] rk808-regulator rk808-regulator: there is no dvs1 gpio
[    0.272654] DCDC_REG1: supplied by vdd_5v
[    0.275652] DCDC_REG2: supplied by vdd_5v
[    0.277551] DCDC_REG3: supplied by vdd_5v
[    0.278161] DCDC_REG4: supplied by vdd_5v
[    0.279830] LDO_REG1: supplied by vcc_io_33
[    0.283177] LDO_REG2: supplied by vcc_io_33
[    0.286601] LDO_REG3: supplied by vdd_5v
[    0.298835] rk808-rtc rk808-rtc: registered as rtc0
[    0.299489] i2c i2c-1: of_i2c: modalias failure on /i2c@ff160000/usb
[    0.300057] i2c i2c-1: Failed to create I2C device for /i2c@ff160000/usb
[    0.305948] energy_model: pd0: hertz/watts ratio non-monotonically decreasing: em_cap_state 1 >= em_cap_state0
[    0.308670] sdhci: Secure Digital Host Controller Interface driver
[    0.309224] sdhci: Copyright(c) Pierre Ossman
[    0.309609] Synopsys Designware Multimedia Card Interface Driver
[    0.310873] dwmmc_rockchip ff500000.dwmmc: IDMAC supports 32-bit address mode.
[    0.311546] dwmmc_rockchip ff500000.dwmmc: Using internal DMA controller.
[    0.312146] dwmmc_rockchip ff500000.dwmmc: Version ID is 270a
[    0.312705] dwmmc_rockchip ff500000.dwmmc: DW MMC controller at irq 25,32 bit host data width,256 deep fifo
[    0.313614] vcc_sd: supplied by vcc_io_33
[    0.330410] mmc_host mmc0: Bus speed (slot 0) = 400000Hz (slot req 400000Hz, actual 400000HZ div = 0)
[    0.344009] sdhci-pltfm: SDHCI platform and OF driver helper
[    0.345720] ledtrig-cpu: registered to indicate activity on CPUs
[    0.346495] hidraw: raw HID events driver (C) Jiri Kosina
[    0.347132] usbcore: registered new interface driver usbhid
[    0.347649] usbhid: USB HID core driver
[    0.349373] NET: Registered protocol family 10
[    0.351004] Segment Routing with IPv6
[    0.351414] NET: Registered protocol family 17
[    0.351891] bridge: filtering via arp/ip/ip6tables is no longer available by default. Update your scripts to load br_netfilter if you need this.
[    0.353043] 8021q: 802.1Q VLAN Support v1.8
[    0.370928] clk: failed to reparent clk_mac2io to gmac_clk: -22
[    0.371474] clk: failed to reparent clk_mac2io_ext to gmac_clk: -22
[    0.372064] rk_gmac-dwmac ff540000.ethernet: IRQ eth_wake_irq not found
[    0.372646] rk_gmac-dwmac ff540000.ethernet: IRQ eth_lpi not found
[    0.373427] rk_gmac-dwmac ff540000.ethernet: PTP uses main clock
[    0.374140] rk_gmac-dwmac ff540000.ethernet: clock input or output? (input).
[    0.374809] rk_gmac-dwmac ff540000.ethernet: TX delay(0x24).
[    0.375310] rk_gmac-dwmac ff540000.ethernet: RX delay(0x18).
[    0.375818] rk_gmac-dwmac ff540000.ethernet: integrated PHY? (no).
[    0.376408] rk_gmac-dwmac ff540000.ethernet: cannot get clock clk_mac_speed
[    0.377015] rk_gmac-dwmac ff540000.ethernet: clock input from PHY
[    0.382593] rk_gmac-dwmac ff540000.ethernet: init for RGMII
[    0.383348] rk_gmac-dwmac ff540000.ethernet: User ID: 0x10, Synopsys ID: 0x35
[    0.383988] rk_gmac-dwmac ff540000.ethernet:         DWMAC1000
[    0.384454] rk_gmac-dwmac ff540000.ethernet: DMA HW capability register supported
[    0.385117] rk_gmac-dwmac ff540000.ethernet: RX Checksum Offload Engine supported
[    0.385773] rk_gmac-dwmac ff540000.ethernet: COE Type 2
[    0.386234] rk_gmac-dwmac ff540000.ethernet: TX Checksum insertion supported
[    0.386898] rk_gmac-dwmac ff540000.ethernet: Wake-Up On Lan supported
[    0.387486] rk_gmac-dwmac ff540000.ethernet: Normal descriptors
[    0.388021] rk_gmac-dwmac ff540000.ethernet: Ring mode enabled
[    0.388535] rk_gmac-dwmac ff540000.ethernet: Enable RX Mitigation via HW Watchdog Timer
[    0.389407] libphy: stmmac: probed
[    0.433505] mmc_host mmc0: Bus speed (slot 0) = 150000000Hz (slot req 150000000Hz, actual 150000000HZ div = 0)
[    0.435316] random: fast init done
[    0.460677] rk808-rtc rk808-rtc: setting system clock to 2020-11-02T05:33:07 UTC (1604295187)
[    0.463284] Waiting for root device PARTUUID=5452574f-02...
[    0.594451] usb 4-1: new SuperSpeed Gen 1 USB device number 2 using xhci-hcd
[    1.596436] dwmmc_rockchip ff500000.dwmmc: Successfully tuned phase to 89
[    1.597089] mmc0: new ultra high speed SDR104 SDHC card at address aaaa
[    1.599147] mmcblk0: mmc0:aaaa SC32G 29.7 GiB
[    1.602423]  mmcblk0: p1 p2 p3
[    1.624742] EXT4-fs (mmcblk0p2): warning: mounting unchecked fs, running e2fsck is recommended
[    1.643935] EXT4-fs (mmcblk0p2): mounted filesystem without journal. Opts: (null)
[    1.644697] VFS: Mounted root (ext4 filesystem) on device 179:2.
[    1.646571] Freeing unused kernel memory: 1664K
[    1.662363] Run /sbin/init as init process
[    1.709971] init: Console is alive
[    1.765318] kmodloader: loading kernel modules from /etc/modules-boot.d/*
[    1.781371] uhci_hcd: USB Universal Host Controller Interface driver
[    1.783125] kmodloader: done loading kernel modules from /etc/modules-boot.d/*
[    1.792062] init: - preinit -
[    1.930140] random: jshn: uninitialized urandom read (4 bytes read)
[    1.952282] random: jshn: uninitialized urandom read (4 bytes read)
[    1.971124] random: hexdump: uninitialized urandom read (5 bytes read)
[    6.127485] mount_root: mounting /dev/root
[    6.130555] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)
[    6.131323] mount_root: loading kmods from internal overlay
[    6.139289] kmodloader: loading kernel modules from //etc/modules-boot.d/*
[    6.140703] kmodloader: done loading kernel modules from //etc/modules-boot.d/*
[    6.194522] block: attempting to load /etc/config/fstab
[    7.137407] EXT4-fs (mmcblk0p3): recovery complete
[    7.138838] EXT4-fs (mmcblk0p3): mounted filesystem with ordered data mode. Opts:
[    7.148734] mount_root: switched to extroot
[    7.202658] EXT4-fs (mmcblk0p1): mounted filesystem without journal. Opts: (null)
[    7.212003] urandom-seed: Seeding with /etc/urandom.seed
[    7.234757] procd: - early -
[    7.783519] procd: - ubus -
[    7.796766] urandom_read: 5 callbacks suppressed
[    7.796776] random: ubusd: uninitialized urandom read (4 bytes read)
[    7.837267] random: ubusd: uninitialized urandom read (4 bytes read)
[    7.838767] procd: - init -
[    7.957637] urngd: v1.0.2 started.
[    7.976712] kmodloader: loading kernel modules from /etc/modules.d/*
[    7.981838] random: crng init done
[    8.008055] usbcore: registered new interface driver r8152
[    8.016435] xt_time: kernel timezone is -0000
[    8.029377] PPP generic driver version 2.4.2
[    8.030813] NET: Registered protocol family 24
[    8.039871] kmodloader: done loading kernel modules from /etc/modules.d/*
[    8.179778] usb 4-1: reset SuperSpeed Gen 1 USB device number 2 using xhci-hcd
[    8.215908] r8152 4-1:1.0 (unnamed net_device) (uninitialized): Invalid ether addr 00:00:00:00:00:00
[    8.216745] r8152 4-1:1.0 (unnamed net_device) (uninitialized): Random ether addr ca:e2:a0:45:5c:aa
[    8.252004] r8152 4-1:1.0 eth1: v1.10.11
[   10.673084] br-lan: port 1(eth1) entered blocking state
[   10.673100] br-lan: port 1(eth1) entered disabled state
[   10.673501] device eth1 entered promiscuous mode
[   10.675310] br-lan: port 1(eth1) entered blocking state
[   10.675331] br-lan: port 1(eth1) entered forwarding state
[   10.739227] rk_gmac-dwmac ff540000.ethernet eth0: PHY [stmmac-0:01] driver [RTL8211E Gigabit Ethernet]
[   10.750424] rk_gmac-dwmac ff540000.ethernet eth0: No Safety Features support found
[   10.750453] rk_gmac-dwmac ff540000.ethernet eth0: PTP not supported by HW
[   10.751091] rk_gmac-dwmac ff540000.ethernet eth0: configuring for phy/rgmii link mode
[   11.682902] br-lan: port 1(eth1) entered disabled state
[   11.717046] r8152 4-1:1.0 eth1: Promiscuous mode enabled
[   11.717319] r8152 4-1:1.0 eth1: carrier on
[   11.717837] br-lan: port 1(eth1) entered blocking state
[   11.717852] br-lan: port 1(eth1) entered forwarding state
[   12.706413] IPv6: ADDRCONF(NETDEV_CHANGE): br-lan: link becomes ready
[   13.826955] rk_gmac-dwmac ff540000.ethernet eth0: Link is Up - 100Mbps/Full - flow control off
[   13.827046] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[   18.985803] pppoe-wan: renamed from ppp0





Notes

Space for additional notes, links to forum threads or other resources.

…

