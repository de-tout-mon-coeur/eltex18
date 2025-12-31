init
1. subl init.c
2. arm-linux-gnueabihf-gcc -static init.c -o init
3. echo "init" | cpio -o -H newc | gzip > initramfs.cpio.gz
4. QEMU_AUDIO_DRV=none qemu-system-arm -M vexpress-a9 -kernel zImage -dtb vexpress-v2p-ca9.dtb -initrd initramfs.cpio.gz -append "console=ttyAMA0" -nographic

static/dynamic
1. ARCH=arm make defconfig
2. ARCH=arm make menuconfig
3. ARCH=arm make -j 4
4. ARCH=arm DESTDIR=$PWD/_install make install
5. find . | cpio -o -H newc | gzip > initramfs.cpio.gz
6. QEMU_AUDIO_DRV=none qemu-system-arm -M vexpress-a9 -kernel zImage -dtb vexpress-v2p-ca9.dtb -initrd initramfs.cpio.gz -append "console=ttyAMA0 rdinit=/bin/ash" -nographic
7. mkdir proc
8. mount -t proc proc proc
