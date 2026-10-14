# 1. Install Required Packages
On Ubuntu 22.04/24.04:
```bash
sudo apt update
sudo apt install -y gawk wget git-core diffstat unzip texinfo gcc \
build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
xz-utils debianutils iputils-ping python3-git python3-jinja2 \
libsdl1.2-dev xterm rsync curl
```
# 2. Create Workspace
```bash
mkdir ~/yocto
cd ~/yocto
```
# 3. Clone TI Layer Setup
Solve SSL errors:
```bash
git config --global http.sslVerify false
```
Clone the repo:
```bash
git clone https://git.ti.com/git/arago-project/oe-layersetup.git
cd oe-layersetup
```
# 4.Set the build folder according to the MACHINE and Version
```bash
MACHINE=j721e-evm ./oe-layertool-setup.sh -f configs/arago-scarthgap-config.txt
```
# 5.Install UTF-8
```bash
sudo locale-gen en_US.UTF-8
sudo update-locale LANG=en_US.UTF-8
```
```bash
export LANG=en_US.UTF-8
```
```bash
locale
```
You may see something like this
```bash
LANG=en_US.UTF-8
LANGUAGE=en_IN:en
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=en_US.UTF-8
```
# 6.Set board environment in within your current shell session
go to build folder and set the environment
```bash
cd ~/yocto/oe-layersetup/build
source conf/setenv
```



## Replace the board name on the 'setenv' file
```bash
sudo gedit conf/local.conf
```

Change `#MACHINE ?= "arago"` to `MACHINE ?= "j721e-evm"`

## Install this tar=1.34+dfsg-1build3
```bash
sudo apt install tar=1.34+dfsg-1build3
```
> [!CAUTION]
> Install tar=1.34+dfsg-1build3 otherwise build may fail!.


Check the bitbake version
```bash
bitbake --version
```
And it will show something like this 
```bash
BitBake Build Tool Core version 2.8.1
```
# 7. Build the image


```bash
bitbake tisdk-default-image -fk 
```
or 

```bash
bitbake core-image-minimal -fk 
```

`-f`: force
`-k`: Keep going


It will start something like below..
```bash
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ bitbake tisdk-default-image -fk
NOTE: Started PRServer with DBfile: /home/krishnawa/yocto/oe-layersetup/build/cache/prserv.sqlite3, Address: 127.0.0.1:35003, PID: 767713
Loading cache: 100% |#######################################################################################################################################################################| Time: 0:00:06
Loaded 11094 entries from dependency cache.
NOTE: Resolving any missing task queue dependencies
NOTE: Resolving any missing task queue dependencies
NOTE: Resolving any missing task queue dependencies
NOTE: Resolving any missing task queue dependencies
NOTE: Resolving any missing task queue dependencies
NOTE: Resolving any missing task queue dependencies

Build Configuration (mc:default):
BB_VERSION           = "2.8.1"
BUILD_SYS            = "x86_64-linux"
NATIVELSBSTRING      = "ubuntu-22.04"
TARGET_SYS           = "aarch64-oe-linux"
MACHINE              = "j721e-evm"
DISTRO               = "arago"
DISTRO_VERSION       = "2025.01"
TUNE_FEATURES        = "aarch64"
TARGET_FPU           = ""
meta-ti-foundational 
meta-ti-ml           = "HEAD:650d7affad2d00137f922ca4579c3cee211e4d12"
meta-arago-distro    
meta-arago-extras    
meta-arago-test      = "HEAD:b9c8be5cceb1b67c6d14a871fe20e599542999f1"
meta-qt6             = "HEAD:c7d71f4cece4d1fe3110c945b9bd40ca82960e05"
meta-virtualization  = "HEAD:17ac21e7d7f6f40a87618b22278b63bcfa14dbf2"
meta-networking      
meta-python          
meta-oe              
meta-gnome           
meta-filesystems     
meta-multimedia      = "HEAD:89a01c3d9ad1f8fce6aeb4dd0e694cfa28d42099"
meta-lts-mixins      = "HEAD:a8046d5ec53b1856169ac795aa87cb0d5db84c04"
meta-ti-extras       
meta-beagle          
meta-ti-bsp          = "HEAD:802cd01e783aeec00af81d99cd242f20ede3adc9"
meta-arm             
meta-arm-toolchain   = "HEAD:a81c19915b5b9e71ed394032e9a50fd06919e1cd"
meta-clang           = "HEAD:731488911f55ebfe746068512b426351192f82f2"
meta                 = "HEAD:1fbd9eddbdf0da062df0510cabff6f6ee33d5752"


Build Configuration:
BB_VERSION           = "2.8.1"
BUILD_SYS            = "x86_64-linux"
NATIVELSBSTRING      = "ubuntu-22.04"
TARGET_SYS           = "arm-oe-eabi"
MACHINE              = "j721e-evm-k3r5"
DISTRO               = "arago"
DISTRO_VERSION       = "2025.01"
TUNE_FEATURES        = "arm armv7a vfp thumb callconvention-hard"
TARGET_FPU           = "hard"
meta-ti-foundational 
meta-ti-ml           = "HEAD:650d7affad2d00137f922ca4579c3cee211e4d12"
meta-arago-distro    
meta-arago-extras    
meta-arago-test      = "HEAD:b9c8be5cceb1b67c6d14a871fe20e599542999f1"
meta-qt6             = "HEAD:c7d71f4cece4d1fe3110c945b9bd40ca82960e05"
meta-virtualization  = "HEAD:17ac21e7d7f6f40a87618b22278b63bcfa14dbf2"
meta-networking      
meta-python          
meta-oe              
meta-gnome           
meta-filesystems     
meta-multimedia      = "HEAD:89a01c3d9ad1f8fce6aeb4dd0e694cfa28d42099"
meta-lts-mixins      = "HEAD:a8046d5ec53b1856169ac795aa87cb0d5db84c04"
meta-ti-extras       
meta-beagle          
meta-ti-bsp          = "HEAD:802cd01e783aeec00af81d99cd242f20ede3adc9"
meta-arm             
meta-arm-toolchain   = "HEAD:a81c19915b5b9e71ed394032e9a50fd06919e1cd"
meta-clang           = "HEAD:731488911f55ebfe746068512b426351192f82f2"
meta                 = "HEAD:1fbd9eddbdf0da062df0510cabff6f6ee33d5752"

NOTE: Tainting hash to force rebuild of task /home/krishnawa/yocto/oe-layersetup/sources/meta-arago/meta-arago-distro/recipes-core/images/tisdk-default-image.bb, do_build                  | ETA:  0:00:02
WARNING: /home/krishnawa/yocto/oe-layersetup/sources/meta-arago/meta-arago-distro/recipes-core/images/tisdk-default-image.bb:do_build is tainted from a forced run                          | ETA:  0:00:02
Sstate summary: Wanted 6480 Local 149 Mirrors 0 Missed 6331 Current 262 (2% match, 6% complete)############################################################################                 | ETA:  0:00:00
Initialising tasks: 100% |##################################################################################################################################################################| Time: 0:00:13
NOTE: Executing Tasks
```
