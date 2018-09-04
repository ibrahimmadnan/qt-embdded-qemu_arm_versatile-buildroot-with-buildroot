# qt-embdded-qemu_arm_versatile-buildroot-with-buildroot
configuration for buildroot to use qt creator on virtual qemu using qemu_arm_versatile 


download buildroot

# mkdir at91sam_buildroot

# git clone git://git.buildroot.net/buildroot

# cd buidroot

check the latest stable release
#git tag

checkout the latest
# git checkout 2018.05

clean any old configuration
# make defconfig
# qemu_arm_versatile_defconfig
# make clean

# make qemu_arm_versatile_defconfig

copy the provided .config file into the buildroot folder

if you need to edit you can use 

# make menuconfig 

now run from buildroot folder

# make 

compiling will take about 45 minuets

you can run the qemu using the following command 

# qemu-system-arm -M versatilepb -kernel output/images/zImage -dtb output/images/versatile-pb.dtb -drive file=output/images/rootfs.ext2,if=scsi,format=raw -append "root=/dev/sda console=ttyAMA0,115200" -serial mon:stdio -net nic,model=rtl8139 -net user,hostfwd=tcp::10022-:22

login to your image shell using root , without any password

now you can set password using the following command
# passwd

You can use ssh from host using the command 

# ssh root@localhost -p10022

