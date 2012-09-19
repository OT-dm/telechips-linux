telechips-kernel-3.0.8
======================

Telechips Kernel 3.0.8 Source Code - https://www.telechips.com/technical_support/kor/opensource/opensource_list.asp

Cloned from https://github.com/cnxsoft/telechips-linux.git
Instructions for building CX01 kernel - http://www.cnx-software.com/2012/07/18/building-linux-kernel-3-0-8-for-telechips-tcc8925-mini-pcs-cx-01-z900-tizzbird-n1
Instructions for packaging custom FWDN firmware, including packaging boot image with custom kernel - http://www.cnx-software.com/2012/08/22/how-to-create-a-custom-android-firmware-for-cx-01/

Disclaimer:
    All operations performed with devices are responsibility of respective owners of their devices.
    I'm providing all things as is. The testing was performed only on my CX01(8G, 7/27 ROM) device and can't be considered as comprehencive.

This is the WIP release, to let other developers working on custom kernel. It fixes 4 roadblocks which don't let originally published kernel to load with 7/27 ROM:
    - mismatching vermagic not allows to load stock modules;
    - mismatching version of MALI driver and stock userspace graphics binaries;
    - not configured framebuffer address and size for MALI driver;
    - stock wlan.ko is loaded only if kernel built with CONFIG_MMC_TCC_SUPPORT_EMMC=y

Working:
    - basic apps (Antutu, Opera, Market) except applications using VPU video acceleration.

Not working:
    - video acceleration (Youtube, BS Player, etc) - fail on VPU initialization.

Next step:
    - fix VPU (planning to enable /dev/vpu character device though no evidence that it was enabled in stock kernel);

Possible improvements:
    - build kernel with speed rather than size optimizations;
    - switch to Linaro toolchain;
    - experiment with 20000000 limit set for connecting SDIO Wifi card;
    - move to upper 3.0.x or 3.x kernel code.


Instruction for building kernel from this git and preparing it for device:
    1. make tcc8925st_cx01_donglehs_defconfig
    2. make ARCH=arm CROSS_COMPILE=arm-eabi- NOLOCALVERSION=y -j2 uImage
       //NOLOCALVERSION=y introduced to keep vermagic '3.0.8-tcc' instead of '3.0.8-tcc+'
    3. Create boot image(see complete cnxsoft guide)
    4. Load created boot image to your device (via custom recovery, FWDN image, fastboot, etc).

CWM recovery for loading this kernel can be built from https://github.com/olexiyt/cm_device_telechips_tcc8920st