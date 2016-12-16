# Google Pixel manual updates
This guide will document how to manually update a rooted (doesn't have to be) Google Pixel while keeping data (including how to re-root / install TWRP after taking the update).

> #### NOTE!
> * This guide assumes you are familiar with command-line terminals and adb / fastboot
> * Text within brackets such as **_[filename]_** are variables to be replaced at your discretion


## Prerequisites
### Hardware
* Google Pixel needing OTA (can be rooted and/or using TWRP, doesn't matter)
* USB C cable to connect Pixel to Linux / OSX (You can do the same on a Windows machine, but the commands will be slightly different)

### Software
* Most recent versions of adb & fastboot (get them via [Android Studio](https://developer.android.com/studio/index.html))

## Setup
### Collect necessary external files

#### Android factory images
We're using the full Android factory image for this method (not the OTA file). Note the differences between Google Pixel "sailfish" and Google Pixel XL "marlin".
* Download the latest factory image for your device located [here](https://developers.google.com/android/images) *(at the time of this writing, the latest version for the Google Pixel was 7.1.1 (NMF26O, Dec 2016))*
* Extract the ZIP, take note of the file ```flash-all.sh```

#### SuperSU
* Download the flashable ZIP [here](https://plus.google.com/+Chainfire/posts/jpR76YEgaM9?sfc=true) *(at the time of this writing, the latest version was SuperSU v2.78 SR5)*

#### TWRP
Download the latest version for your Pixel (minimum required version is alpha2 - NOTE: RC1 does not currently work with flashing SuperSU v2.78 SR5). Make sure to download both the ```[filename].img``` and ```[filename].zip```
* [Google Pixel](https://twrp.me/devices/googlepixel.html)
* [Google Pixel XL](https://twrp.me/devices/googlepixelxl.html)

Copy the TWRP ```[filename].zip``` and SuperSU ```[filename].zip``` to your Pixel.

## Installation
### Android factory images
#### Updating the Pixel with the new factory image
* Open ```flash-all.sh``` with a text editor. Note the commands located inside:
```bash
fastboot flash bootloader [bootloader-filename].img
fastboot reboot-bootloader
sleep 5
fastboot flash radio [radio-filename].img
fastboot reboot-bootloader
sleep 5
fastboot -w update [image-filename].zip
```
* You'll be running these commands manually, removing the wipe option ```-w``` from the last command
* Connect Pixel to computer, ensure USB debugging is enabled on the device
* Open a terminal window and navigate to the folder where the extracted Android factory image is.
* Start by rebooting the Pixel into the bootloader
```bash
adb reboot-bootloader
```
* Flash the new bootloader
```bash
fastboot flash bootloader [bootloader-filename].img
```
* Reboot the bootloader
```bash
fastboot reboot-bootloader
```
* Flash the new radio
```bash
fastboot flash radio [radio-filename].img
```
* Reboot the bootloader
```bash
fastboot reboot-bootloader
```
* Flash the new image (remove ```-w``` to keep data)
```bash
fastboot update [image-filename].zip
```
* Once this completes, reboot the Pixel
```bash
fastboot reboot
```

At this point, if you had root or TWRP before, it's gone now (expected). You should have a stock Pixel running the image downloaded with all your saved applications etc.

### TWRP
#### Flashing TWRP (temporarily or permanent)
There are two installation options. You can 'permanently' install TWRP over the stock recovery, or just temporarily boot into it to root the Pixel.

* In the terminal, navigate to the directory where the TWRP ```[filename].img``` exists
* Start by rebooting the Pixel into the bootloader
```bash
adb reboot-bootloader
```
* Temporarily boot TWRP
```bash
fastboot boot [filename].img
```
* Once TWRP is booted, swipe across to allow modifications
* Temporary booting into TWRP is complete, you can proceed to rooting the device now
* **OPTIONAL** - Install the TWRP ```[filename].zip``` on the Pixel via TWRP to permanently replace the stock recovery with TWRP

### SuperSu
#### Rooting the Pixel
At this point you should still be in TWRP.
* Install the SuperSU ```[filename].zip``` on the Pixel via TWRP to grant root access on the device
* Reboot the device

## Wrap up
And that's about it. You should have the latest Android factory image installed with root access, all without losing any data or settings. However, any changes made that required root previously (such as build.prop edits) will need to be replicated.
