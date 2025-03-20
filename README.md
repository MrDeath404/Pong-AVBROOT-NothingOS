# AVBROOT OTA Package & Tutorial For Nothing Phone (2) ( Pong )
- Allows to use rooted operating system and keep bootloader locked

# Original repositories
- [AVBROOT](https://github.com/chenxiaolong/avbroot) by [chenxiaolong](https://github.com/chenxiaolong)
- [KernelSU Next](https://github.com/KernelSU-Next/KernelSU-Next) by [rifsxd](https://github.com/rifsxd)
- [Nothing Archive](https://github.com/spike0en/nothing_archive) by [spike0en](https://github.com/spike0en) <br>
Thanks them all for their work, to show respect go it to their repositories and leave a star <br>
This is only tutorial how to use AVBROOT tool made by [chenxiaolong](https://github.com/chenxiaolong)

# What is bootloader ?
- Bootloader is the frist program that starts with cpu boots
- The main goal of bootloader is to load system images that have valid root of trust, otherwise the phone will displace that the system has been corrupted

# Why you need to unlock bootloader ?
- When bootloader is locked you are not able to flash/boot any other operating system that doesn't have valid root of trust. That's why we going to unlock it and then load custom root of trust

# Why you may want to relock bootloader ?
- The main resson is when the bootloader is lock no one is allowed to format or change operating system on your phone so simplest answer is to protect your data

# When your device can become unbootable (hard bricked)
- Device will be unbootable (hard bricked) when you will modify any of installed images (by root for example) so don't update KernelSU Next by it's manager

# What is root and why it might be useful for you ?
- In Linux root is first created account with all available privilages, in short root can do everything to the system
- On android platform root allows you to do almost every thing. Here is an example:
    - Installing adblocks (like [Adaway](https://github.com/AdAway/AdAway))
    - Installing modules (ports of applications, sound enhancers, ui improvements)
    - Modifing system partiting (via modules)
    - Tweaking system (improving preformance in some cases)
    - Having better controll
    - Allows you to use terminal emulators (like [Termux](https://github.com/termux/termux-app)) on root level

# KernelSU Next Manager is not detecting root ?
- If you facing this problem try use system for sometime, reboot it or reopen/reinstall KernelSU Manager

# Now we can get started
1. Frist thing you should do is to read [warning](#warning), [requirements](#requirements) and [important information](#important-information)
2. If you already have done it and want to apply update please read about [applying OTA update](#applying-OTA-update) if not go to [frist installation](#frist-Installation) or if you want to go back to fully stock operating system go to [removing AVBROOT patches](#removing-avbroot-patches).
    - You can also make your own one by reading AVBROOT tutorial [own ota updates/build](#how-to-make-own-patched-ota)

# Warning
- **I'm not responsible for any devices that are hard bricked by following this procedure**
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

# Requirements
- A brain
- Nothing Phone (2)
- PC with the lastest platform tools
- USB cable C-A

# Important information
- AVBROOT is not providing google play integracy on any level so you will need to install following module:
    - [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext) by [Dr-TSNG](https://github.com/Dr-TSNG)
    - [Play Integracy Fix](https://github.com/chiteroman/PlayIntegrityFix) by [chiteroman](https://github.com/chiteroman)
    - [TrickyStore](https://github.com/5ec1cff/TrickyStore) by [5ec1cff](https://github.com/5ec1cff) (with valid keyboxes to pass strong integracy)
- Check if you can sideload OTA package without installation error otherwise you won't be able to update without wiping super which is not allowed in lock state
- Phone will display message at every boot that it's running modificated operating system at lock state
- **It is recommended to leave following setting always on:**
```
System -> Developer options -> OEM unlocking
```

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

5. Do not forget to wipe super empty (or you won't be able to update in future):
```sh
fastboot wipe-super super_empty.img
```

6. Download OTA zip from [releases](#releases), move it to repo folder and extract:
```sh
avbroot ota extract --input ota.zip.patched --directory extracted --fastboot --all
```

7. Set ANDROID_PRODUCT_OUT variable inside your console:
```sh
set ANDROID_PRODUCT_OUT=extracted
```

8. Now move following files into extracted folder inside repo folder:
    - android-info.txt
    - fastboot-info.txt

9. Flash all system images (including firmware):
```sh
fastboot flashall --skip-reboot
```

10. Reboot phone into bootloader again to apply custom root of trust:
```sh
fastboot reboot bootloader
```

11. Apply custom root of trust
```sh
fastboot reboot bootloader
fastboot erase avb_custom_key
fastboot flash avb_custom_key avb_pkmd.bin
```
The device might says ( but it's okey ):
```
Warning: skip copying avb_custom_key image avb footer (avb_custom_key partition size: 0, avb_custom_key image size: 1032)
```

12. To make sure everything is ready reboot phone into system grant root permision for adb and execute this command via adb shell:
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

13. Now we can finally lock the bootloader (frist reboot into bootloader):
```sh
fastboot flashing lock
```

14. After system boots turn off auto updates in developer options

# How to make own patched ota
1. Frist you will need to generate own keys to sign ota and to load them into bootloader:
```sh
avbroot key generate-key -o avb.key
avbroot key generate-key -o ota.key
```

3. Do not forget passwords that you gave to these files or you won't be able to make next steps

4. Now we will convert keys to make bootloader able to read them:
```sh
avbroot key encode-avb -k avb.key -o avb_pkmd.bin
```

5. OTA updates need special cetrificate to sign it with so we will make one:
```sh
avbroot key generate-cert -k ota.key -o ota.crt
```

6. Now download an OTA update (it can be custom os ota but I didn't test this on any)
- To apply patches use this command:
    - If you want to patch with root you will need to get rooted image (prepatched) then you will just add this:
```sh
--prepatched prepatched_boot.img
```
    - If you dont want to have rooted system add this:
```sh
--rootless
```
- Now full command to patch OTA with your own keys:
```sh
avbroot ota patch --input ota.zip --key-avb avb.key --key-ota ota.key --cert-ota ota.crt (root flag)
```

7. Go to [frist installation](#frist-Installation) if you didn't flash your ota before or [applying OTA update](#applying-OTA-update) if you have done it

# Applying OTA update
- Simply by ADB sideload (via recovery). You can do it with following command:
```sh
adb sideload update_package_path.zip
```

# Removing AVBROOT patches
- Simply erase avb_custom_key partition:
```sh
fastboot flash avb_custom_key avb_pkmd.bin
```

# Releases
[NothingOS 3.0-250113-1723 With KernelSU Next](https://mega.nz/file/hhRlnIoD#icU7CNFvF0g-wTx6hnNojtAkNAMenMxldu85RBWuK9U) release date 16.03.2025
[NothingOS 3.0-250304-1717 With KernelSU Next](https://mega.nz/file/ZgZS2YwB#Sc2scZrSkBTx_F3rNEWzZCFzVcFJE2e9OcFkuJ0eQGE) release date 20.03.2025