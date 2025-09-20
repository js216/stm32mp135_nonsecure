# Simplified Boot Process for STM32MP135

This repository presents a Buildroot defconfig and board configuration files
for a simplified boot process of the STM32MP135 evaluation board.

There are two simplification: remove U-Boot, and remove OP-TEE. Refer the the
instructions below to get started, or consult the articles:

- [STM32MP135 Without U-Boot (TF-A Falcon Mode)](https://embd.cc/stm32mp135-without-u-boot)
- OP-TEE: TBD

### STM32MP135 Without U-Boot (TF-A Falcon Mode)

Clone the Buildroot repository. To make the procedure reproducible, let's start
from a fixed commit (latest at the time of this writing):

```
$ git clone https://gitlab.com/buildroot.org/buildroot.git --depth=1
$ cd buildroot
$ git checkout 5b6b80bfc5237ab4f4e35c081fdac1376efdd396
```

Obtain this repository with the patches we need. Copy the defconfig and the
board-specific files into the Buildroot tree.

```
$ git clone git@github.com:js216/stm32mp135_simple.git
$ cd buildroot # NOT stm32mp135_simple
$ git apply ../stm32mp135_simple/patches/add_falcon.patch
$ git apply ../stm32mp135_simple/patches/increase_fip.patch
$ cp ../configs/stm32mp135_simple/stm32mp135f_dk_falcon_defconfig configs
$ cp -r ../board/stm32mp135_simple/stm32mp135f-dk-falcon board/stmicroelectronics
```

Build as usual, but using the new defconfig:

```
$ make stm32mp135f_dk_falcon_defconfig
$ make
```

Flash to the SD card and boot into the new system. You should reach the login
prompt exactly as in the default configuration---but without involving U-Boot

### Non-Secure Boot Process (No OP-TEE)

Work in progress!
