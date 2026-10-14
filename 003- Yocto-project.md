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
# 4.Install UTF-8
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
# 5.Set board environment in within your current shell session
go to build folder and set the environment
```bash
cd ~/yocto/oe-layersetup/build
source conf/setenv
```
Check the bitbake version
```bash
bitbake --version
```
And it will show something like this 
```bash
BitBake Build Tool Core version 2.8.1
```
# 6. 
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ bitbake tisdk-default-image
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
#MACHINE ?= "arago"
SDKMACHINE ?= "x86_64"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ sed -i 's/^MACHINE.*/MACHINE ?= "j721e-evm"/' conf/local.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
#MACHINE ?= "arago"
SDKMACHINE ?= "x86_64"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ locale
locale -a
bitbake --version
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
C
C.utf8
en_AG
en_AG.utf8
en_AU.utf8
en_BW.utf8
en_CA.utf8
en_DK.utf8
en_GB.utf8
en_HK.utf8
en_IE.utf8
en_IL
en_IL.utf8
en_IN
en_IN.utf8
en_NG
en_NG.utf8
en_NZ.utf8
en_PH.utf8
en_SG.utf8
en_US.utf8
en_ZA.utf8
en_ZM
en_ZM.utf8
en_ZW.utf8
POSIX
BitBake Build Tool Core version 2.8.1
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
#MACHINE ?= "arago"
SDKMACHINE ?= "x86_64"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ ls ../sources/meta-ti/meta-ti-bsp/conf/machine/
am335x-evm.conf        am62xx-lp-evm.conf       beagleplay-ti-k3r5.conf  j721s2-evm-k3r5.conf
am335x-hs-evm.conf     am62xx-lp-evm-k3r5.conf  dra7xx-evm.conf          j721s2-hs-evm.conf
am437x-evm.conf        am62xxsip-evm.conf       dra7xx-hs-evm.conf       j721s2-hs-evm-k3r5.conf
am437x-hs-evm.conf     am62xxsip-evm-k3r5.conf  include                  j722s-evm.conf
am57xx-evm.conf        am64xx-evm.conf          j7200-evm.conf           j722s-evm-k3r5.conf
am57xx-hs-evm.conf     am64xx-evm-k3r5.conf     j7200-evm-k3r5.conf      j742s2-evm.conf
am62axx-evm.conf       am65xx-evm.conf          j7200-hs-evm.conf        j742s2-evm-k3r5.conf
am62axx-evm-k3r5.conf  am65xx-evm-k3r5.conf     j7200-hs-evm-k3r5.conf   j784s4-evm.conf
am62dxx-evm.conf       am65xx-hs-evm.conf       j721e-evm.conf           j784s4-evm-k3r5.conf
am62dxx-evm-k3r5.conf  am65xx-hs-evm-k3r5.conf  j721e-evm-k3r5.conf      j784s4-hs-evm.conf
am62lxx-evm.conf       am68-sk.conf             j721e-hs-evm.conf        j784s4-hs-evm-k3r5.conf
am62pxx-evm.conf       am68-sk-k3r5.conf        j721e-hs-evm-k3r5.conf   omapl138-lcdk.conf
am62pxx-evm-k3r5.conf  am69-sk.conf             j721e-sk.conf
am62xx-evm.conf        am69-sk-k3r5.conf        j721e-sk-k3r5.conf
am62xx-evm-k3r5.conf   beagleplay-ti.conf       j721s2-evm.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ nano conf/local.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ nano conf/local.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ echo 'MACHINE ?= "j721e-evm"' >> conf/local.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
#MACHINE ?= "arago"
SDKMACHINE ?= "j721e-evm"
MACHINE ?= "j721e-evm"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ nano conf/local.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
#MACHINE ?= "arago"
SDKMACHINE ?= "x86_64"
MACHINE ?= "j721e-evm"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ grep MACHINE conf/local.conf
MACHINE ?= "j721e-evm"
SDKMACHINE ?= "x86_64"
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ ls ../sources/meta-ti/meta-ti-bsp/conf/machine/
am335x-evm.conf        am62dxx-evm.conf       am62xx-lp-evm-k3r5.conf  am65xx-hs-evm-k3r5.conf  dra7xx-hs-evm.conf      j721e-hs-evm.conf        j722s-evm.conf           omapl138-lcdk.conf
am335x-hs-evm.conf     am62dxx-evm-k3r5.conf  am62xxsip-evm.conf       am68-sk.conf             include                 j721e-hs-evm-k3r5.conf   j722s-evm-k3r5.conf
am437x-evm.conf        am62lxx-evm.conf       am62xxsip-evm-k3r5.conf  am68-sk-k3r5.conf        j7200-evm.conf          j721e-sk.conf            j742s2-evm.conf
am437x-hs-evm.conf     am62pxx-evm.conf       am64xx-evm.conf          am69-sk.conf             j7200-evm-k3r5.conf     j721e-sk-k3r5.conf       j742s2-evm-k3r5.conf
am57xx-evm.conf        am62pxx-evm-k3r5.conf  am64xx-evm-k3r5.conf     am69-sk-k3r5.conf        j7200-hs-evm.conf       j721s2-evm.conf          j784s4-evm.conf
am57xx-hs-evm.conf     am62xx-evm.conf        am65xx-evm.conf          beagleplay-ti.conf       j7200-hs-evm-k3r5.conf  j721s2-evm-k3r5.conf     j784s4-evm-k3r5.conf
am62axx-evm.conf       am62xx-evm-k3r5.conf   am65xx-evm-k3r5.conf     beagleplay-ti-k3r5.conf  j721e-evm.conf          j721s2-hs-evm.conf       j784s4-hs-evm.conf
am62axx-evm-k3r5.conf  am62xx-lp-evm.conf     am65xx-hs-evm.conf       dra7xx-evm.conf          j721e-evm-k3r5.conf     j721s2-hs-evm-k3r5.conf  j784s4-hs-evm-k3r5.conf
krishnawa@krishnawa-virtual-machine:~/yocto/oe-layersetup/build$ bitbake tisdk-default-image
NOTE: Started PRServer with DBfile: /home/krishnawa/yocto/oe-layersetup/build/cache/prserv.sqlite3, Address: 127.0.0.1:35461, PID: 4093
