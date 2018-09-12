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

you can now send command using ssh

update framebuffer resolution

fbset -fb /dev/fb0 -g 800 600 800 600 16
