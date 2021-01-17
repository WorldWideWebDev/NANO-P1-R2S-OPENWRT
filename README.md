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

