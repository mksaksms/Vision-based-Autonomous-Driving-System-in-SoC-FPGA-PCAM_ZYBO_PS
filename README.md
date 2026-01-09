# PCAM_ZYBO_PS


PCAM_ZYBO_PS is a Zynq-based embedded vision project that interfaces a Pcam camera module with the ZYBO development board, leveraging the Processing System (PS) for control and data handling. The project focuses on capturing real-time image data from the camera, configuring the imaging pipeline, and transferring frames between the programmable logic and the ARM processing system for further processing, display, or storage.

This project serves as a practical reference for camera sensor integration on Zynq SoCs, demonstrating PS–PL interaction, AXI-based data movement, and embedded software control. It is suitable for applications such as computer vision prototyping, image acquisition systems, and hardware–software co-design experiments on FPGA-based platforms.

Petalinux and Vivado version 19.1 



Modules : 
vdmadriver 
Application : 
uiotools (must needed - will be loaded automically while booting)
initcamera(must needed - start before videotest app)
videotest (testing software)


Added device tree : 
Device Tree 

HDF file (design_wrapper_1.hdf) vivado 2019.1 
bit file - final.bit vivado 2019.1 


sudo apt-get install -y gcc git make net-tools libncurses5-dev tftpd zlib1g-dev libssl-dev
flex bison libselinux1 gnupg wget diffstat chrpath socat xterm autoconf libtool tar unzip
texinfo zlib1g-dev gcc-multilib build-essential libsdl1.2-dev libglib2.0-dev zlib1g:i386
screen pax gzip
```
petalinux-config --get-hw-description /home/kawser/Peta19/camera 

petalinux-create -t apps -n uiotools
petalinux-create -t apps -n initcamera
petalinux-create -t apps -n videotest --template c --enable
petalinux-create -t modules -n vdmadriver
petalinux-config -c rootfs
petalinux-build -c uiotools 
petalinux-build -c videotest 
petalinux-build -c vdmadriver
petalinux-build -c initcamera
petalinux-build 
petalinux-package --boot --format BIN --fsbl ./images/linux/zynq_fsbl.elf --fpga ./images/linux/final.bit --u-boot --force
```

To clean any software 
petalinux-build -c uiotools -x cleanall 

To test the software in Petalinux OS : 
check /bin folder 

cd bin 
ls 



Issues : 
I found that if petalinux-create -t apps -n videotest --template c --enable template is not fixed then 
 
 
 To take the picture : 
 ./videotest /dev/video kawser
 
 It will create a kawser.ppm in the /home/root/kawser.ppm
 
 mount /dev/mmcblk0p1 /mnt/ 
 cp /home/root/kawser.ppm /mnt
 
 I have added a sh script in the file named takepicture.sh 
 
 
