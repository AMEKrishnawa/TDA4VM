# 1. Find out the source file path
```text
/home/krishnawa/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/board-support/ti-linux-kernel-6.12.57+git-ti/arch/arm64/boot/dts/ti/k3-j721e-sk.dts
```


# 2. Go to kernal directory
open terminal here.

```bash
cd ~/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/board-support/ti-linux-kernel-6.12.57+git-ti
```
# 3. Check the .config file is there or not
```bash
ls arch/arm64/configs/
```
# 4. If the file is not there, use this command
```bash
make ARCH=arm64 defconfig
```
## Check the the config file
```bash
ls -l .config
```
---
# 5. open the file 
```bash
/home/krishnawa/ti-processor-sdk-linux-adas-j721e-evm-11_02_01_03/board-support/ti-linux-kernel-6.12.57+git-ti/arch/arm64/boot/dts/ti/k3-j721e-sk.dts
```
# 6. Add this code here

```bash
/{

}
```
here paste this 

```c
   gpio_leds {
    compatible = "gpio-leds";
    pinctrl-names = "default";
    pinctrl-0 = <&user_led2_pins_default>;
 
    user_led1 {
        label = "user_led1";
        gpios = <&main_gpio0 71 GPIO_ACTIVE_HIGH>;
        default-state = "off";
            };
    
    user_led2 {
    label = "user_led2";
    gpios = <&main_gpio0 64 GPIO_ACTIVE_HIGH>;
    default-state = "off";
        };
    
 
    };
```


Add this below on `&main_pmx0`
```c
&main_pmx0 {
    user_led2_pins_default: user-led2-default-pins {
        pinctrl-single,pins = <
            J721E_IOPAD(0x104, PIN_OUTPUT, 7) /* PRG0_PRU1_GPO1.GPIO0_64 */
        >;
    };
};

[Use this link to enable GPIO64 (on board LED)](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1686130/tda4vm-j721e-evm-gpio0_64-configured-via-gpio-leds-but-it-always-stays-low-and-cannot-be-controlled/6498365)

```

---
# 7. Compile the 'k3-j721e-sk.dts' file to 'k3-j721e-sk.dtb'
```bash
make ARCH=arm64 dtbs -j$(nproc)
```
# 8. Copy the .dtb file to /tftpboot/ti location
```bash
sudo cp arch/arm64/boot/dts/ti/k3-j721e-sk.dtb /tftpboot/ti
```
---
Use TFTP to boot the board
---

# On Target


# 1. Verify the LEDs label are showing correctly
```bash 
ls /sys/class/leds/
```
# 2. Example commands for controlling user_led1 (high)
```bash
echo 1 > /sys/class/leds/user_led1/brightness
```
