# J721EXSKG01EVM Ethernet Boot Setup Guide

**Board:** J721EXSKG01EVM (SK-TDA4VM)  
**Host:** Ubuntu VM on VMware  (22.04)
**Purpose:** Boot Linux over Ethernet using TFTP/NFS

---

# 1. Working Network Configuration

## EVM

```bash
IP Address : 192.168.1.110
Netmask    : 255.255.255.0
MAC        : 34:08:e1:59:de:ec
```

## Ubuntu VM

```bash
IP Address : 192.168.1.100
Netmask    : 255.255.255.0
```

---

# 2. Important findings
Problem discovered during debugging:
```bash
Ping and TFTP failed whenever Windows Wi-Fi was enabled.
```
Reason:
```bash
VMware Bridged mode was automatically binding to the Wi-Fi adapter
instead of the Ethernet adapter.
```
Solution:
```bash
Disable Wi-Fi
OR
Configure VMware VMnet0 to bridge only to the Ethernet adapter.
```
# 3. Verify Host Network

Check Ubuntu IP:

```bash
ip addr
```

Expected:

```bash
192.168.1.100/24
```
# 4. U-Boot Network Configuration

Verify:


```bash
printenv ipaddr
printenv serverip
printenv ethaddr
```
Expected:


```bash
ipaddr=192.168.1.110

serverip=192.168.1.100
```
Set manually if needed:


```bash
setenv ipaddr 192.168.1.110

setenv serverip 192.168.1.100
```
# 5. Verify Ethernet Link

Check:


```bash
net list
```
Expected:


```bash
eth0 active
```
Ping host:


```bash
ping 192.168.1.100
```
Expected:


```bash
link up on port 1

host 192.168.1.100 is alive
```

# 6. TFTP Server Installation

Install:

```bash
sudo apt update
sudo apt install tftpd-hpa
```
# 7. TFTP Configuration

File:

```bash
/etc/default/tftpd-hpa
```
Contents:

```bash
TFTP_USERNAME="tftp"

TFTP_DIRECTORY="/tftpboot"

TFTP_ADDRESS=":69"

TFTP_OPTIONS="--secure"
```

IMPORTANT:

Initially TFTP was configured as:

```bash
/srv/tftp
```
but all files were stored in:

```bash
/tftpboot
```

This caused:

```bash
TFTP error: 'File not found'
```

Fixed by changing:

```bash
TFTP_DIRECTORY="/tftpboot"
```
Restart:

```bash
sudo systemctl restart tftpd-hpa
```

Verify:

```bash
sudo systemctl status tftpd-hpa
```
# 8. TFTP Root Directory

Create:


```bash
sudo mkdir -p /tftpboot
```
Permissions:

```bash
sudo chmod 777 /tftpboot
```

# 8. Test File

Create:

```bash
echo hello > /tftpboot/test.txt
```
Test from EVM:

```bash
tftpboot 0x82000000 test.txt
```

Expected:

```bash
Bytes transferred = 6
```
# 9. SDK Files Stored In TFTP Directory

Kernel:


```bash
Image-j721e-evm.bin
```
Device Tree:


```bash
k3-j721e-sk.dtb
```

Location:

```bash
/tftpboot
````
# 10. Verify DTB Download

U-Boot:

```bash
tftpboot 0x88000000 k3-j721e-sk.dtb
```

Successful output:

```bash
Bytes transferred = 123194
```
# 11. Verify Kernel Download

U-Boot:


```bash
tftpboot 0x82000000 Image-j721e-evm.bin
```

Successful output:

```bash
Bytes transferred = 22391296
```

# 12. Network Boot Status

Verified Working:


```bash
✓ Ethernet Link
✓ Ping EVM -> Host
✓ TFTP Server
✓ DTB Download
✓ Kernel Download
✓ VMware Configuration Identified
```

# 13. Select Kernel Boot Method

```bash 
setenv bootargs 'console=ttyS2,115200n8 root=/dev/nfs rw nfsroot=192.168.1.100:/home/krishnawa/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/targetNFS,tcp,v3,nolock ip=192.168.1.110::192.168.1.100:255.255.255.0:j721e:eth0:off'
```
# 13. Boot Linux

After kernel and DTB load successfully:


```bash
booti 0x82000000 - 0x88000000
```

or

```bash
booti ${kernel_addr_r} - ${fdt_addr_r}
```



