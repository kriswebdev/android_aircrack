# Aircrack-ng for Android
This reposiroty is a port of the Aircrack-ng suite (except scripts) for Android.
This port is done by KrisWebDev and is not "afiliated" with the Aircrack-ng.org team.

# Aircrack-ng
Aircrack-ng is an 802.11 WEP and WPA-PSK keys cracking program that can recover
keys once enough data packets have been captured. It implements the standard FMS
attack along with some optimizations like KoreK attacks, as well as the
all-new PTW attack, thus making the attack much faster compared to other WEP
cracking tools.

It can attack WPA1/2 networks with some advanced methods or simply by brute force.
It can also fully use a multiprocessor system to its full power in order
to speed up the cracking process.


# Building for Android

## Pre-requisites

You should use CyanogenMod build system or any similar build system that include those libraries in an /external folder.

 * OpenSSL development package
 * SQLite development package `>= 3.3.17` (3.6.X version or better is recommended): `libsqlite3-devel`

Your device WiFi firmware and driver need to support monitor mode. As of early 2014, the only mass-market compatible devices are the devices having a Broadcom 4329 or 4330 chipsets.

 * Kernel driver in Monitor Mode: check "bcmon" on a search engine.
 * WiFi chipset firmware in Monitor Mode: check "bcmon" on a search engine.

Your device needs to have the Wireless Extensions tools `iwconfig`, `iwconfig` and `iwpriv` installed in `/system/xbin` or `/system/bin` (or any other locations listed in osdep/linux.c).

## Preparing the build environment

 * Follow [Cyanogenmod build guide](http://wiki.cyanogenmod.org/w/Build_Guides) for your device but stop before "brunch".
 * Copy this Aircrack for Android repository content to a directory named "aircrack-ng" in CyanogenMod source root "external" folder.

## Compilating

The following commands have to be run from the CyanogenMod android source directory (croot).

 * Compilation:

    `. external/aircrack-ng/make_aircrack.sh`

 * Push binaries to the device (through adb, USB debugging mode must be enabled on the device):

     `. external/aircrack-ng/push_aircrack.sh`

 * Re-compile and push:

     `mmmp external/aircrack-ng`

 * Checking (provided that monitor mode is enabled on your device and that interface name is eth0):

    `adb shell airodump-ng eth0`


# Using precompiled binaries

Android:
 * Get these Wireless Extesions binaries (can be found in this repo /bin folder): `iwconfig`, `iwconfig` and `iwpriv`
 * Get Aircrack-ng for Android binaries from this repo /bin folder
 * Install Android SDK tools on your computer
 * Connect your Android device in USB debugging mode and run from your computer:
 
    `adb root`
    `adb remount`
    `adb push some-binary /system/xbin/`

# Documentation

Documentation, tutorials, ... can be found on http://www.aircrack-ng.org

See also manpages and the forum.

For further information check the [README](README) file.

Support Aircrack-ng for Android is done on a yet-to-come XDA thread.
