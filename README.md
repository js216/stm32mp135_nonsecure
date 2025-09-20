### Quick Start

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
$ git clone git@github.com:js216/stm32mp135_nonsecure.git
$ cd buildroot # NOT stm32mp135_nonsecure
$ git apply ../stm32mp135_nonsecure/patches/add_falcon.patch
$ git apply ../stm32mp135_nonsecure/patches/increase_fip.patch
$ cp ../stm32mp135_nonsecure/stm32mp135f_dk_nonsecure_defconfig configs
$ cp -r ../stm32mp135_nonsecure/stm32mp135f-dk-nonsecure board/stmicroelectronics
```

Build as usual, but using the new defconfig:

```
$ make stm32mp135f_dk_nonsecure_defconfig
$ make
```

Flash to the SD card and boot into the new system. You should reach the login
prompt exactly as in the default configuration---but without involving U-Boot or
OP-TEE.
