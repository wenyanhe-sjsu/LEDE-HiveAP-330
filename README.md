# LEDE-HiveAP-330

Bringup for the Aerohve HiveAP 330 Access Point on LEDE!

**NOTE: This repo is NO LONGER MAINTAINED as these changes were merged upstream. Refer to https://github.com/lede-project/source/commit/f2b7d9dc1ca3d33f14961cf2885639f4f9e8965e and enjoy the official LEDE nightles!**

## Building

### Build Only

`./build.sh`

### Modify Configs and Build

`./build.sh modify`

Note that you will need to run a modify on the first compile to select the `Freescale MPC85xx` target, `P1020` subtarget, and `HiveAP-330` as the target profile in the LEDE menuconfig.

## Accessing U-Boot

1. Hookup to the Console port (speed 9600) and power on the device and enter the bootloader. Note you may need to enter a password of `administrator` or `AhNf?d@ta06` if prompted.

## Booting

1. Access a U-Boot prompt using the information provided above.
2. Setup a local PXE server with your boot files. In the below example, our PXE server is at 192.168.1.101
3. At the U-Boot shell, enter the following:

  ```
  dhcp;
  tftpboot 0x1000000 192.168.1.101:lede-mpc85xx-p1020-hiveap-330-initramfs.zImage;
  tftpboot 0x6000000 192.168.1.101:lede-mpc85xx-p1020-hiveap-330.fdt;
  bootm 0x1000000 - 0x6000000;
  ```

## Flashing

1. Follow the Booting instructions above to boot into an initramfs build of LEDE.
2. Once booted, SCP over a copy of the sysupgrade image from your PC. In the below example, your PC has the IP of 192.168.1.101 and your local user is "user". Be sure to update the command to match your file location and local user.

	```
	scp user@192.168.1.101:~/lede-mpc85xx-p1020-hiveap-330-sysupgrade.img ~
	```

3. Now that we have a copy of the sysupgrade file, we can flash it. Note that this process will take a few minutes due to the size of the image.

	```
	sysupgrade ~/lede-mpc85xx-p1020-hiveap-330-sysupgrade.img
	```

4. Reboot, and enjoy LEDE!

## TPM Notes

While support for the TPM has been added, at this time the password for the TPM is unknown. If you want to reset the TPM you will need to do this manually by resetting the TPM, reloading the modules, and reviewing <https://cryptotronix.com/2014/08/28/compliance_mode/>. If you need `tpm_assertpp`, a pre-compiled version can be downloaded [here](https://servernetworktech.com/uploads/files/hiveap-330/tpm_assertpp.zip).

Note that if you do this, the process is irreversible and **YOU WILL NOT BE ABLE TO GO BACK TO STOCK!** This is due to the fact the stock firmware relies on the information stored within the TPM. Because of this, the reset process will **NOT** be documented.

Technical specs of the TPM can be found at:

- <http://www.atmel.com/Images/Atmel-5295S-TPM-AT97SC3204-LPC-Interface-Datasheet-Summary.pdf>
- <http://csrc.nist.gov/groups/STM/cmvp/documents/140-1/140sp/140sp2014.pdf>

## To Do

### HiveAP-330

- You tell me!

## Working

### HiveAP-330

- NAND
- Ethernet
- WiFi
- LEDs
- Reset button
- USB
- TPM
- Sysupgrade

## Notice

No promises this won't brick your unit, and no promises that this will even work!

## Shout-outs

A big thank you to Sun for donating the hardware!
