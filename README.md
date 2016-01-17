# Introduction
## Aircrack-ng for Android
This repository is a port of the Aircrack-ng 1.2-beta2 suite (except scripts) for Android. It works directly on top of Android.
This port is done by [KrisWebDev](https://github.com/kriswebdev) and is not "affiliated" with the Aircrack-ng.org team.

## Aircrack-ng
> Aircrack-ng is an 802.11 WEP and WPA-PSK keys cracking program that can recover keys once enough data packets have been captured. It implements the standard FMS attack along with some optimizations like KoreK attacks, as well as the PTW attack, thus making the attack much faster compared to other WEP cracking tools.

# Running Aircrack-ng on Android (precompiled)

## Pre-requisites

 1. Device with **WiFi chipset, firmware & driver that support monitor-mode**
   * As of early 2016, only mass-market compatible devices are thoses having dedicated **Broadcom 4329** or **Broadcom 4330** WiFi chipsets (**Samsung Galaxy S1, Samsung Galaxy S2, Nexus 7, Huawei Honor**). Bcmon team has developed firmware and driver hacks for these chipsets. More recent devices have WiFi digital signal processed by the ARM CPU (Qualcomm or Samsung) and there is no publicily knowed monitor mode hacks for these devices at this time.
   * Otherwise, go look for **USB WiFi adapter** known to provide WiFi monitor-mode and injection support on Android.
 2. **Wireless extensions** enabled in Android kernel
  * That's normally bundled with the loaders/kernels below.
 3. Monitor-mode **firmware & driver loader**
   * Broadcom 4329: Bcmon won't load on CyanogenMod > v7 due to the move from bcm4329 driver to bcmdhd. For Galaxy S1, use [PwnAir](http://forum.xda-developers.com/showthread.php?t=2760170) on a KitKat ROM (or port the open source PwnAir kernel to more recent ROM following PwnAir kernel build instructions at the end of the XDA thread).
   * Broadcom 4330: Use [bcmon app](http://bcmon.blogspot.com/) (not maintained anymore by their owners) to load the monitor-mode firmware and driver.
   * USB WiFi adapter that supports monitor-mode: This completely depends on your USB WiFi adapter and is out of scope of this README. You will need to find and build the driver, along with your ROM kernel, from source. Otherwise you would probably face the *magic version* mismatch issue. As Android kernel is a Linux kernel afterall, you should succeed in compiling the driver from the Linux driver source. Either find a nice HowTo for compiling your USB WiFi adapter driver on a defined Android ROM, or struggle with the driver build guide of your Android ROM. Note that some guides use a chrooted Ubuntu or Kali as an alternative to building a pure Android driver.
 4. [**Android SDK**](https://developer.android.com/sdk/index.html#Other) platform-tools installed on your PC (for install)

## Install

* Load the monitor-mode firmware/driver
  * Automated: Install [bcmon app](http://bcmon.blogspot.com/) (bcm4330) or [PwnAir kernel+app](http://forum.xda-developers.com/showthread.php?t=2760170) (Samsung Galaxy S1 with KitKat ROM) and load the monitor-mode firmware/driver.
  * Manual: [LD_PRELOAD the driver](http://forum.xda-developers.com/showthread.php?t=2405208) (bcm4330) or [port PwnAir kernel](http://forum.xda-developers.com/showthread.php?t=2760170) (bcm4329, check XDA thread build instructions section)
* Install the [wireless extensions binaries](https://github.com/kriswebdev/android_wireless_tools/tree/master/bin) and [aircrack-ng for android binaries](https://github.com/kriswebdev/android_aircrack/tree/master/bin) on the Android device `/system/xbin/` folder:
```shell
    adb root
    adb remount
    adb push some-binary /system/xbin/
```

## Run

Check your wireless interface status (should be in "Mode: Monitor"):
```shell
adb root
adb shell iwconfig
```

Check Airodump is working:
    `adb shell airodump-ng eth0`

Provided your wireless interface is eth0.

If it is working, then check the Aircrack documentation for HowTo.

# Building Aircrack-ng on Android

There is no need to build Aircrack-ng yourself unless you're paranoïd about the binaries I provide. Android Aircrack-ng binaries should work on any Android phone. The hard part about running Aircrack-ng on Android is to load a monitor-mode WiFi firmware & driver that works for your phone or USB WiFi adapter: that's where you should focus your efforts instead.

## Pre-requisites

### Firmware/driver pre-requisite

Same pre-requisites applies as for Running. You still need to have a monitor-mode WiFi kernel/driver installed on your Android system prior to using Aicrack for Android.

### Preparing the build environment

Instructions are made for CyanogenMod platform build system, as it includes all the necessary libraries and tools.

> Warning: Compilation has not been tested on Android NDK system build alone, without all the platform tools. Building only with Android NDK instead of CyanogenMod platform build system would require you to have have at least the following sources located in an folder called "external" (check Android.mk):
>  * Aircrack-ng for Android (android_aircrack)
>  * OpenSSL development package (openssl)
>  * SQLite development package `>= 3.3.17` (3.6.X version or better is recommended): `libsqlite3-devel`
>  * zlib (or change the Andorid.mk flags to use -LDLIB)

 * Follow [Cyanogenmod build guide](http://wiki.cyanogenmod.org/w/Build_Guides) for your device but stop before "brunch". You need several GB of disk space and a good Internet connection.
 * Copy this Aircrack-ng for Android repository content to a directory named "aircrack-ng" in CyanogenMod source root "external" folder.

### Building wireless tools binaries

If you also want to build the [Android wireless tools](https://github.com/kriswebdev/android_wireless_tools/) instead of using the Android wireless tools [precompiled binaries](https://github.com/kriswebdev/android_wireless_tools/tree/master/bin), then download and put the [Android wireless tools](https://github.com/kriswebdev/android_wireless_tools/) in CyanogenMod "external" folder and run from the CM source root (`croot`): 

```shell
. build/envsetup.sh
breakfast galaxysmtd
export USE_CCACHE=1
mka iwconfig
mka iwpriv
adb root
adb remount
adb push $OUT/system/bin/iwconfig /system/xbin/
adb push $OUT/system/bin/iwpriv /system/xbin/
```

And so on for all tools listed in [Android wireless tools Android.mk](https://github.com/kriswebdev/android_wireless_tools/blob/master/Android.mk). Replace galaxysmtd by your device CyanogenMod name.

## Building Aicrack-ng binaries for Android

The following commands have to be run from the CyanogenMod android source directory (croot).

 * Edit `. external/aircrack-ng/make_aircrack.sh` and replace "galaxysmtd" with your device [CyanogenMod code](http://wiki.cyanogenmod.org/w/Devices), should it have any impact at all.

 * Compilation:

    `. external/aircrack-ng/make_aircrack.sh`

 * Push binaries to the device (through adb, USB debugging mode must be enabled on the device):

     `. external/aircrack-ng/push_aircrack.sh`

 * Re-compile and push:

     `mmmp external/aircrack-ng`

 * Checking (provided that monitor mode is enabled on your device and that interface name is eth0):

    `adb shell airodump-ng eth0`

# Documentation

## Aircrack-ng official documentation 
> Documentation, tutorials, ... can be found on (http://www.aircrack-ng.org)[http://www.aircrack-ng.org]
> 
> See also manpages and the forum.
> 
> For further information check the [README](README) file.

## Aircrack-ng for Android

Support Aircrack-ng for Android is done on [XDA PwnAir thread](http://forum.xda-developers.com/showthread.php?t=2760170) Q&A section or on the [GitHub repo](https://github.com/kriswebdev/android_aircrack/).
