# 1. GCC ToolChain Setup
Before compiling any of the sources referenced in this document, set the cross compiler paths to the toolchains packaged in the Processor SDK

```bash
export SDK_INSTALL_DIR="$HOME/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/"
export CROSS_COMPILE_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-"
export SYSROOT_64="${SDK_INSTALL_DIR}/linux-devkit/sysroots/aarch64-oe-linux"
export CC_64="${CROSS_COMPILE_64}gcc --sysroot=${SYSROOT_64}"
export CROSS_COMPILE_32="${SDK_INSTALL_DIR}/k3r5-devkit/sysroots/x86_64-arago-linux/usr/bin/arm-oe-eabi/arm-oe-eabi-"
export ENV_SETUP_64="${SDK_INSTALL_DIR}/linux-devkit/environment-setup-aarch64-oe-linux"
```

> [!NOTE]
> Yocto-built SDK Toolchains The Processor SDK Linux SK TDA4VM package contains cross compile toolchains for the **ARMv8 (linux-devkit)** and **ARMv7 (k3r5-devkit)** architectures.

# 2. Environment-setup script
- When cross-compiling packages that use configuration tools and autotools there are many settings that are required to make sure that the proper cross-compile libraries are used.
- To use the environment-setup script you only need to source it. This is as simple as:

```bash
source ${ENV_SETUP_64}
```

# 3.Testing the Cross compiler

- Make a HelloWorld Folder
```bash
mkdir HelloWorld
```
- open the folder
```bash
cd HelloWorld
```
- create a example .c file 
```bash
sudo nano helloworld.c
```
paste this 
```c
#include <stdio.h>

int main() {
    printf ("Hello World from TI!!!\n");
    return 0;
}
```
and save the file.

## Compile the c file to generate an executable for the target architecture, using one of the below 2 methods:
- ### **Compile Directly** : Cross-compile the helloworld.c file using the cross-compile toolchain directly

```bash
${CROSS_COMPILE_64}gcc --sysroot=${SYSROOT_64} helloworld.c -o helloworld
```
you may see an executable file now..

- ### **Using the environement setup script** : Cross-compile after sourcing the setup script

```bash
source ${ENV_SETUP_64}
${CC} helloworld.c -o helloworld
```
## Run the executable
After the above steps are run you should now have a helloworld executable in your directory that has been compiled for the ARM. A simple way to check this is to run the “file” command. 

```bash
file helloworld
```
It should return an output similar to below:
```bash
helloworld: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=57fd909ade6f1a3739adc5410c81189a713ff172, for GNU/Linux 5.15.0, with debug_info, not stripped
```

# 4.ARM toolchains
It is strongly recommended to use the toolchain in the Processor SDK which is the version tested for TI platforms. In case the Processor SDK is not used, the toolchain can be downloaded and the toolchain path can be set as follows.


```bash
cd ~
COMPILER_PATH=/opt/arm-toolchain
mkdir -p $COMPILER_PATH
wget -c https://developer.arm.com/-/media/Files/downloads/gnu/11.3.rel1/binrel/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-linux-gnueabihf.tar.xz -O - | tar -xv -J -C $COMPILER_PATH
wget -c https://developer.arm.com/-/media/Files/downloads/gnu/11.3.rel1/binrel/arm-gnu-toolchain-11.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz -O - | tar -xv -J -C $COMPILER_PATH
export CROSS_COMPILE_64=$COMPILER_PATH/arm-gnu-toolchain-11.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
export CROSS_COMPILE_32=$COMPILER_PATH/arm-gnu-toolchain-11.3.rel1-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-
export CC_64="${CROSS_COMPILE_64}gcc"
export CC_32="${CROSS_COMPILE_32}gcc"
```
