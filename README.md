# Simplified Boot Process for STM32MP135

This repository presents a Buildroot defconfig and board configuration files
for a simplified boot process of the STM32MP135 evaluation board.

There are two simplification: remove U-Boot, and remove OP-TEE. Refer the the
instructions below to get started, or consult the articles for more information:

- [STM32MP135 Without U-Boot (TF-A Falcon Mode)](https://embd.cc/stm32mp135-without-u-boot)
- OP-TEE: TBD

### STM32MP135 Without U-Boot (TF-A Falcon Mode)

Clone the Buildroot repository. To make the procedure reproducible, let's start
from a fixed commit (latest at the time of this writing):

```
$ git clone https://gitlab.com/buildroot.org/buildroot.git
$ cd buildroot
$ git checkout 5d2abdc66ca687c926e1c4546c12aca7f7d72ca4
```

Obtain this repository with the patches we need. Copy the defconfig and the
board-specific files into the Buildroot tree.

```
$ git clone git@github.com:js216/stm32mp135_simple.git
$ cd buildroot # NOT stm32mp135_simple
$ git apply ../stm32mp135_simple/patches/add_falcon.patch
$ git apply ../stm32mp135_simple/patches/increase_fip.patch
$ cp ../stm32mp135_simple/configs/stm32mp135f_dk_falcon_defconfig configs
$ cp -r ../stm32mp135_simple/board/stm32mp135f-dk-falcon board/stmicroelectronics
```

Build as usual, but using the new defconfig:

```
$ make stm32mp135f_dk_falcon_defconfig
$ make
```

Flash to the SD card and boot into the new system. You should reach the login
prompt exactly as in the default configuration---but without involving U-Boot

### Non-Secure Boot Process (No OP-TEE)

*WARNING: Work in progress!*

Start by cloning Buildroot as above. However, this time we check out a different
sequence of patches and board files:

```
$ git clone https://gitlab.com/buildroot.org/buildroot.git
$ cd buildroot
$ git checkout 3645e3b781be5cedbb0e667caa70455444ce4552

$ git clone git@github.com:js216/stm32mp135_simple.git
$ cd buildroot # NOT stm32mp135_simple

$ git apply ../stm32mp135_simple/patches/add_falcon.patch

$ cp ../stm32mp135_simple/configs/stm32mp135f_dk_nonsecure_defconfig configs
$ cp -r ../stm32mp135_simple/board/stm32mp135f-dk-nonsecure board/stmicroelectronics
```

Now build:

```
$ make stm32mp135f_dk_nonsecure_defconfig
$ make
```
