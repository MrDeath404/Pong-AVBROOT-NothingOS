# AVBROOT OTA Package & Tutorial For Nothing Phone (2) ( Pong )
avbroot allows locking phone's bootloader with root or custom operating system ( in this case only with kernelsu next)

# Orginal repos
[avbroot](https://github.com/chenxiaolong/avbroot)

[kernelsu next](https://github.com/KernelSU-Next/KernelSU-Next)

# Warning
**I'm not responsible for any devices that get hard bricked by following this process**

This process requires formatting data so don't forget to make backup

If this process goes wrong the phone will become unbootable into following modes:

- Recovery mode ( including fastboot )
- Operating system

Bootloader mode **should** be still bootable so make sure you didn't uncheck following settings:

- Settings->Developer Options->Allow OEM unlocking

If this settings is enabled you **should** be safe

If something bad happends and following setting is enabled you **should be able to unlock** bootloader and reflash/sideload operating system

# When the device can get into hard bricked ?
If you will update/flash any of images manually ( not by OTA update ) the device will be unbootable

Do not try to update kernelsu next by it's manager, **every update must be done by OTA update**

# Now we can get started

You can go ahead and clone this repo with following command:

```sh
git clone https://github.com/MrDeath404/Pong-AVBROOT-NothingOS.git
```

Before doing anything further make sure you using lastest version of platform tools

```sh
fastboot --version
```

platform tools 35.0.2 was released in July 2024 which is the latest version for now

# Frist Installation
Download lastest OTA package from releases and place it in avbroot folder

Then execute following command:

```sh
avbroot ota extract --input ota.zip.patched --directory extracted --fastboot --all
```

This command will unpack all images from OTA zip

Now set ANDROID_PRODUCT_OUT variable inside your console:

Linux:
```sh
export ANDROID_PRODUCT_OUT=extracted
```

Windows:
```sh
set ANDROID_PRODUCT_OUT=extracted
```

Now if you want to flash the images you will need to move following files into extracted folder:

- 