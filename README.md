# AVBROOT OTA Package & Tutorial For Nothing Phone (2) ( Pong )
avbroot allows locking phone's bootloader with root or custom operating system ( in this case kernelsu next)

# Orginal repositories
- [avbroot](https://github.com/chenxiaolong/avbroot) by [chenxiaolong ](https://github.com/chenxiaolong)
- [kernelsu next](https://github.com/KernelSU-Next/KernelSU-Next) by [rifsxd](https://github.com/rifsxd)

Big thanks for their work

# Warning
**I'm not responsible for any devices that get hard bricked by following this process**

This process requires formatting data so don't forget to make backup

If this process goes wrong the phone will become unbootable into following modes:

- Recovery mode ( including fastboot )
- Operating system

Bootloader mode **should** be still bootable so make sure you didn't uncheck following setting:

- Settings->Developer Options->Allow OEM unlocking

If this settings is enabled you **should** be safe

If something goes wrong and following setting is enabled you **should be able to unlock** bootloader and reflash/sideload operating system

# Important information

Remember to wipe super:

```sh
fastboot wipe-super super_empty.img
```

Or you won't be able to update in future

Play integracy not working by default so you will need to flash following modules:

- [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext/releases/tag/v1.2.7) by [Dr-TSNG](https://github.com/Dr-TSNG)
- [Play Integracy Fix](https://github.com/chiteroman/PlayIntegrityFix) by [chiteroman](https://github.com/chiteroman)

Big thanks to them for their work

# When the device can get into hard brick ?
If you will update/flash any of images manually ( not via OTA update ) the device will become unbootable

So do not try to update kernelsu next by it's manager

**Every update must be done by OTA update**

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
Download lastest OTA package from releases and place it in repo folder

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

Now you will need to move following files into extracted folder:

- android-info.txt
- fastboot-info.txt

Now reboot your phone to bootloader

If it isn't already unlocked you must unlock it now:

```sh
fastboot flashing unlock
```

Now you can flash all images by executing:

```sh
fastboot flashall --skip-reboot
```

Reboot device to bootloader to apply avbroot:

```sh
fastboot reboot bootloader
```

Erase avb_custom_key partition:

```sh
fastboot erase avb_custom_key
```

Flash new keys:

```sh
fastboot flash avb_custom_key avb_pkmd.bin
```

The device might says ( but it's okey ):

```sh
Warning: skip copying avb_custom_key image avb footer (avb_custom_key partition size: 0, avb_custom_key image size: 1032)
```

Now we will check if the avbroot applied the paches successfully

Reboot phone into system:

```sh
fastboot reboot
```

Install kernelsu next manager

Grant root for adb and execute following command via adb shell:

```sh
su -c 'dmesg | grep libfs_avb'
```

If you didn't get any result redo every step

You should get following result:

```sh
init: [libfs_avb]Returning avb_handle with status: Success
```

If you get that you can now lock bootloader:

```sh
fastboot flashing lock
```

# Updating

Simply use OTA package and sideload it

```sh
adb sideload path_to_ota
```

# How do I remove the paches ?

Unlock bootloader and execute this command:

```sh
fastboot erase avb_custom_key
```

That's it

# KernelSU Next Manager is not detecting root ?

If you facing this problem try use system for sometime, reboot it or reopen/reinstall kernelsu manager

# Releases

- [NothingOS 3.0-250113-1723 With KernelSU Next](https://mega.nz/file/hhRlnIoD#icU7CNFvF0g-wTx6hnNojtAkNAMenMxldu85RBWuK9U) release date 16.03.2025