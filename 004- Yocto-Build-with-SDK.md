# 1. Setup J721E Build Environment
Go to the SDK folder
```bash
cd yocto-build
./oe-layertool-setup.sh -f configs/processor-sdk-analytics/processor-sdk-analytics-selinux-11.01.07.05-config.txt
```
After running this script two folders will created, "source"and "build".

Go to build folder:
```bash
cd build
```


TOOLCHAIN_BASE=$HOME/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux MACHINE=j721e-evm bitbake -k tisdk-default-image


export SDK_INSTALL_DIR=~/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03

export CROSS_COMPILE_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-"

export SYSROOT_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/aarch64-oe-linux"

export CC_64="${CROSS_COMPILE_64}gcc --sysroot=${SYSROOT_64}"



 export CROSS_COMPILE_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-"
 export SYSROOT_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/aarch64-oe-linux"
 export CC_64="${CROSS_COMPILE_64}gcc --sysroot=${SYSROOT_64}"
 export CROSS_COMPILE_32="${SDK_INSTALL_DIR}/k3r5-devkit/sysroots/x86_64-arago-linux/usr/bin/arm-oe-eabi/arm-oe-eabi-"



 COMPILER_PATH=/opt/arm-toolchain
mkdir -p $COMPILER_PATH
wget -c https://developer.arm.com/-/media/Files/downloads/gnu/11.3.rel1/binrel/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-linux-gnueabihf.tar.xz -O - | tar -xv -J -C $COMPILER_PATH
wget -c https://developer.arm.com/-/media/Files/downloads/gnu/11.3.rel1/binrel/arm-gnu-toolchain-11.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz -O - | tar -xv -J -C $COMPILER_PATH
export CROSS_COMPILE_64=$COMPILER_PATH/arm-gnu-toolchain-11.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
export CROSS_COMPILE_32=$COMPILER_PATH/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-
export CC_64="${CROSS_COMPILE_64}gcc"
export CC_32="${CROSS_COMPILE_32}gcc"


On your ubuntu PC, run the command "sudo apt install tar=1.34+dfsg-1build3"

https://community.nxp.com/t5/Other-NXP-Products/base-files-error-when-build-the-yocto-linux/m-p/2398500#M33011