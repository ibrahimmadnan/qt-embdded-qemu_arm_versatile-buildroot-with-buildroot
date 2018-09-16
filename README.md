# qt-embdded-qemu_arm_versatile-buildroot-with-buildroot
configuration for buildroot to use qt creator on virtual qemu using qemu_arm_versatile 


*execute the following 

#mkdir qemu_arm_versatile_qt

#cd qemu_arm_versatile_qt

#git clone https://github.com/ibrahimmadnan/qt-embdded-qemu_arm_versatile-buildroot-with-buildroot.git

*download buildroot
#git clone git://git.buildroot.net/buildroot

#cd buildroot

check the latest stable release
#git tag

checkout the latest
#git checkout 2018.05

*clean any old configuration

#make defconfig
#make clean
#make qemu_arm_versatile_defconfig

copy the provided .config file into the buildroot folder
#cp ../qt-embdded-qemu_arm_versatile-buildroot-with-buildroot/.config .



if you need to edit you can use 

#make menuconfig 

to build now run 

#make 

compiling will take about 45 minuets

you can run the qemu using the following command 

#qemu-system-arm -M versatilepb -kernel output/images/zImage -dtb output/images/versatile-pb.dtb -drive file=output/images/rootfs.ext2,if=scsi,format=raw -append "root=/dev/sda console=ttyAMA0,115200" -serial mon:stdio -net nic,model=rtl8139 -net user,hostfwd=tcp::10022-:22

login to your image shell using root , without any password

now you can set password using the following command
#passwd

# Testing the output
from qemu terminal run 

#cd usr/lib/qt/examples/gui/analogclock/
#./analogclock -platform linuxfb

it should display analog clock on qemu emulated display


You can use ssh from host using the command 

#ssh root@localhost -p10022

<<<<<<< HEAD


you can now send command using ssh

you can now send command using ssh

update framebuffer resolution

fbset -fb /dev/fb0 -g 800 600 800 600 16
------------------------------------------------------------------------------

If you need to create bridge network to provide separate ip for the qemu guest you have to do :

* enable ability to run dhcp during boot of qemu image.
#make linux-menuconfig

go to network support> TCP/IP networking > kernel level auto configuration

enable it.

* enable e1000 intel ethernet network driver which will be emulated via qemu

go to Device Drivers > Network device support > Ethernet driver support > intel devices 

enable all intel devices

---------------------------------------------------------------------------------


run in buildroot path 

#qemu-system-arm -M versatilepb -kernel output/images/zImage -dtb output/images/versatile-pb.dtb -drive file=output/images/rootfs.ext2,if=scsi,format=raw -append "root=/dev/sda console=ttyAMA0,115200,ip=dhcp" -serial stdio -device e1000,netdev=network0,mac=52:22:00:d1:55:01 -netdev tap,id=network0,ifname=tap0,script=no,downscript=no

if you need to pass ctrl+c to the qemu use :

# qemu-system-arm -M versatilepb -kernel output/images/zImage -dtb output/images/versile-pb.dtb -drive file=output/images/rootfs.ext2,if=scsi,format=raw -append "root=/dev/sda console=ttyAMA0,115200,ip=dhcp" -serial mon:stdio -device e1000,netdev=network0,mac=52:22:00:d1:55:01 -netdev tap,id=network0,ifname=tap0,script=no,downscript=no

------------------------------------

before doing that you will have to create bridge and tap interfaces on your host device :

* create bridge on host system

# sudo ip link add br0 type bridge

* clear the ip address of the host system

# sudo ip addr flush dev eno1        (in some cases use eth0 instead of eno1)

* connect the host ethernet connection to the new bridge 

# sudo ip link set eno1 master br0 

* create new tap interface on the current user called tap0

# sudo ip tuntap add dev tap0 mode tap user $(whoami)

* add that new tap to be connected to the bridge

# sudo ip link set tap0 master br0

* make sure everything is up :

# sudo ip link set dev br0 up
# sudo ip link set dev tap0 up

* better to restart the network service using 
#sudo /etc/init.d/networking restart

* update dhcp using 
# sudo dhclient
#
-------------------------------------------------------------------------------

TODO : -upload automatic script to configure the network on host
       - create patch file to configure buildroot .config and make new defconfig

=======


