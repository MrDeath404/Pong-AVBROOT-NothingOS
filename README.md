# AVBROOT OTA Package & Tutorial For Nothing Phone (2) ( Pong )
- Allows to use rooted operating system and keep bootloader locked

# Requirements
- A brain
- Nothing Phone (2)
- PC with the lastest platform tools
- USB cable C-A

# What is bootloader ?
- Bootloader is the frist program that starts with cpu boots
- The main goal of bootloader is to load system images that have valid root of trust, otherwise the phone will displace that the system has been corrupted

# Why you need to unlock bootloader ?
- When bootloader is locked you are not able to flash/boot any other operating system that doesn't have valid root of trust. That's why we going to unlock it and then load custom root of trust

# What is root and why it might be useful for you ?
- In Linux root is first created account with all available privilages, in short root can do everything to the system
- On android platform root allows you to do almost every thing. Here is an example:
    - Installing adblocks (like [Adaway](https://github.com/AdAway/AdAway))
    - Installing modules (ports of applications, sound enhancers, ui improvements)
    - Modifing system partiting (via modules)
    - Tweaking system (improving preformance in some cases)
    - Having better controll
    - Allows you to use terminal emulators (like [Termux](https://github.com/termux/termux-app)) on root level

# Original repositories
- [AVBROOT](https://github.com/chenxiaolong/avbroot) by [chenxiaolong](https://github.com/chenxiaolong)
- [KernelSU Next](https://github.com/KernelSU-Next/KernelSU-Next) by [rifsxd](https://github.com/rifsxd)
- [Nothing Archive]() by [spike0en](https://github.com/spike0en)

# Warning
- I'm not responsible for any devices that are hard bricked by following this procedure
- This process require data formating so do not forget to take a backup
    - You can take backup with [Swift Backup](https://www.swiftapps.org/) (root is required to make full backup)
- When something goes wrong the device **might** become unbootable into following modes:
    - recovery
    - fastbootd
    - operating system
- Bootloader **should be** still bootable so do not forget to leave this setting on:
```
System -> Developer options -> OEM unlocking
```
- If following setting is turned on the bootloader can be unlocked in anytime

# Important information
- AVBROOT is not providing google play integracy on any level so you will need to install following module:
    - [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext) by [Dr-TSNG](https://github.com/Dr-TSNG)
    - [Play Integracy Fix](https://github.com/chiteroman/PlayIntegrityFix) by [chiteroman](https://github.com/chiteroman)
    - [TrickyStore](https://github.com/5ec1cff/TrickyStore) by [5ec1cff](https://github.com/5ec1cff) (with valid keyboxes to pass strong integracy)
- Check if you can sideload OTA package without installation error otherwise you won't be able to update without wiping super which is not allowed in lock state
- Phone will display message at every boot that it's running modificated operating system at lock state

# When your device can become unbootable (hard briced)
- Device will be unbootable (hard bricked) when you will modify any of installed images (by root for example) so don't update KernelSU Next by it's manager

# How do you apply update ?
- Simply By ADB sideload (via recovery). You can do it with following command:
```sh
adb sideload update_package_path.zip
```

# Now we can get started
1. Frist thing you should do is to read [warning](#warning), [requirements](#requirements) and [important information](#important-information)
2. If you already have done it and wants to apply update please read about [applying OTA update](#how-do-you-apply-update) if not go to [frist installation](#frist-Installation)

# Frist Installation
1. Open console and clone this repositorie:
```sh
git clone https://github.com/MrDeath404/Pong-AVBROOT-NothingOS.git && cd Pong-AVBROOT-NothingOS
```
2. Enable following setting in developer options (developer options can be shown by clicking 5 times on build number in device's info):
```
System -> Developer options -> OEM unlocking
```

3. Reboot phone to bootloader. Press the power button with volume down button when screen goes black release power button but keep holding the volume button untill you'll see bootloader screen

4. If bootloader is not already unlocked you must unlock it now:
```sh
fastboot flashing unlock
```
Device will try to boot into operating system to redo 3th step.

5. Download OTA zip from [releases](#releases), move it to repo folder and extract:
```sh
avbroot ota extract --input ota.zip.patched --directory extracted --fastboot --all
```

6. Set ANDROID_PRODUCT_OUT variable inside your console:
```sh
set ANDROID_PRODUCT_OUT=extracted
```

7. Now move following files into extracted folder inside repo folder:
    - android-info.txt
    - fastboot-info.txt
    - super_empty.img

8. Flash all system images (including firmware):
```sh
fastboot flashall --skip-reboot
```
While this process phone should wipe super automatically but check if it was done with succes in console or do it manually:
```sh
fastboot wipe-super super_empty.img
```

9. Reboot phone into bootloader again to apply custom root of trust:
```sh
fastboot reboot bootloader
```

10. Apply custom root of trust
```sh
fastboot reboot-bootloader
fastboot erase avb_custom_key
fastboot flash avb_custom_key avb_pkmd.bin
```
The device might says ( but it's okey ):
```
Warning: skip copying avb_custom_key image avb footer (avb_custom_key partition size: 0, avb_custom_key image size: 1032)
```

11. To make sure everything is ready reboot phone into system grant root permision for adb and execute this command via adb shell:
```sh
fastboot reboot
```
```sh
su -c 'dmesg | grep libfs_avb'
```
This should return this following message:
```
init: [libfs_avb]Returning avb_handle with status: Success
```

12. Now we can finally lock the bootloader (frist reboot into bootloader):
```sh
fastboot flashing lock
```

# How do you go back and remove applied patches ?
- Simply erase avb_custom_key partition:
```sh
fastboot flash avb_custom_key avb_pkmd.bin
```

# KernelSU Next Manager is not detecting root ?
- If you facing this problem try use system for sometime, reboot it or reopen/reinstall KernelSU Manager

# Releases
[NothingOS 3.0-250113-1723 With KernelSU Next](https://mega.nz/file/hhRlnIoD#icU7CNFvF0g-wTx6hnNojtAkNAMenMxldu85RBWuK9U) release date 16.03.2025