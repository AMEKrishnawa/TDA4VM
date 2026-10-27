
# 1. Setup J721E Build Environment
Go to the SDK folder
```bash
cd $HOME/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/yocto-build
```
# 2.Host Setup - Ubuntu (Recommended)

The following build host packages are required for Ubuntu. The following command will install the required tools on the Ubuntu Linux distribution
```bash
sudo apt-get update
# Install packages required for builds
sudo apt-get -f -y install \
  git build-essential diffstat texinfo gawk chrpath socat doxygen \
  dos2unix python3 bison flex libssl-dev u-boot-tools mono-devel \
  mono-complete curl python3-distutils repo pseudo python3-sphinx \
  g++-multilib libc6-dev-i386 jq git-lfs pigz zstd liblz4-tool \
  cpio file lz4 debianutils iputils-ping python3-git python3-jinja2 \
  python3-subunit locales libacl1 unzip gcc python3-pip python3-pexpect \
  xz-utils wget \
sudo locale-gen en_US.UTF-8
```

```bash
sudo apt install g++
sudo apt install gcc
sudo apt install make        # version 4.3-4.1build1, or
sudo apt install make-guile  # version 4.3-4.1build1
```

**By default Ubuntu uses “dash” as the default shell for /bin/sh. You must reconfigure to use bash by running the following command:**

```bash
sudo dpkg-reconfigure dash
```
>[!IMPORTANT]
>Be sure to select “No” when you are asked to use dash as the default system shell.

# 3. Yocto Layer Configuration
```bash
./oe-layertool-setup.sh -f configs/processor-sdk-analytics/processor-sdk-analytics-11.00.00-config.txt
```
it will build the **source** and **build** folder.

If the following repos are not cloning `https://git.ti.com/git/ti-sdk-linux/meta-tisdk.git` and `https://git.ti.com/git/edgeai/meta-edgeai.git`

Downlod it manually to the `sources` folder

```bash
cd sources 
```
```bash
git clone --single-branch --branch scarthgap \
https://git.ti.com/git/ti-sdk-linux/meta-tisdk.git
```
```bash
git clone --single-branch --branch scarthgap \
https://git.ti.com/git/edgeai/meta-edgeai.git
```
and run again 
```bash
cd ..
./oe-layertool-setup.sh -f configs/processor-sdk-analytics/processor-sdk-analytics-11.00.00-config.txt
```

## Replace the board name on the 'setenv' file
```bash
sudo gedit build/conf/local.conf
```

Change `#MACHINE ?= "arago"` to `MACHINE ?= "j721e-sk"`

# 4.Build the image
Go to build directory
```bash
cd build
```
Set the source environment
```bash
source conf/setenv
```
To keep any issue from downloading packet from git
```bash
git config --global http.sslVerify false
```
Build the image
`MACHINE=<machine> bitbake <target>
`
```bash
MACHINE=j721e-sk bitbake tisdk-base-image -fk 
```
# 5.Additional information

| Target                         | Build Output                                                    | Description                                  |
|--------------------------------|-----------------------------------------------------------------|----------------------------------------------|
| tisdk-core-bundle             | images/<machine>/tisdk-core-bundle-<machine>.tar.xz            | Full SDK                                     |
| tisdk-default-image           | images/<machine>/tisdk-default-image-<machine>.tar.xz          | Target Filesystem                            |
| tisdk-edgeai-image            | images/<machine>/tisdk-edgeai-image-<machine>.tar.xz           | Target EdgeAI Filesystem                     |
| tisdk-adas-image              | images/<machine>/tisdk-adas-image-<machine>.tar.xz             | Target ADAS Filesystem                       |
| tisdk-base-image              | images/<machine>/tisdk-base-image-<machine>.tar.xz             | Minimal Target Filesystem                    |
| tisdk-thinlinux-image         | images/<machine>/tisdk-thinlinux-image-<machine>.tar.xz        | Minimal Target Filesystem with Docker enabled |
| meta-toolchain-arago-tisdk    | sdk/arago-<arago-version>-<architecture>.sh                    | Linux glibc toolchain                        |
| mc:k3r5:meta-toolchain-arago  | sdk/arago-<arago-version>-<architecture>.sh                    | K3R5 baremetal toolchain                     |

## For MACHINE
You can find here
`ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/yocto-build/sources/meta-ti/meta-ti-bsp/conf/machine
`
