
# Alcatel Onetouch Fire E / Alcatel 6015x

msm8610



## Usage

- Enter Recovery Mod via \[Power\] + \[Vol Down\]
- Enter Bootloader via \[Power\] + \[Vol Up\]


The device is using `fastboot` for the flashing process
and `adb` for debugging.

On Linux, install adb and fastboot via:

```bash
sudo apt-get install android-tools-adb android-tools-fastboot
```

The device has the vendor id `05c6`, so you need to add
it to your udev android.rules file:

```bash
cat /etc/udev/rules.d/android.rules;
SUBSYSTEM="usb", ATTR{idVendor}=="05c6", MODE="0666", GROUP="plugdev"
```

After modifying the udev rules file, make sure to `sudo service udev restart`
and re-plugin your device to the physical USB port.


## Root

The root process requires fastboot and adb.
First, boot the recovery image and **DO NOT FLASH** it.


```bash
adb reboot bootloader
sudo fastboot boot custom/recovery.img;
```

- Wait for the boot of the device until you see the Recovery Mod.
- Go to "Install zip" > "Install zip from sideload"

On your computer, sideload the SuperSU binaries now:

```bash
adb sideload custom/SuperSU-v2.46.zip;
```

Wait for the sideload installation process to be completed, then
reboot your phone via `adb reboot`.


## Backup Images

Creating a backup image is not really hard. The first thing you have to know
is that FirefoxOS is very different from the typical Android partition setup:


| Partition Name     | Reference              |
|:-------------------|-----------------------:|
| boot               | /dev/block/mmcblk0p7   |
| firmware (modem)   | /dev/block/mmcblk0p1   |
| system             | /dev/block/mmcblk0p12  |
| persist            | /dev/block/mmcblk0p13  |
|:-------------------|-----------------------:|
| cache              | /dev/block/mmcblk0p14  |
| custpack           | /dev/block/mmcblk0p31  |
| userdata           | /dev/block/mmcblk0p32  |
|:-------------------|-----------------------:|
| scard              | /dev/block/vold/259:1  |
| scard1             | /dev/block/vold/179:65 |
|:-------------------|-----------------------:|


## Backup Procedure

First, you must have followed the Rooting instructions. If you have root, you
can connect to the device via `adb shell` and backup the partitions with the
following instructions.

```bash
# On adb shell
cat /dev/block/platform/msm_sdcc.1/by-name/boot     > /storage/sdcard0/boot.img
cat /dev/block/platform/msm_sdcc.1/by-name/modem    > /storage/sdcard0/firmware.img
cat /dev/block/platform/msm_sdcc.1/by-name/system   > /storage/sdcard0/system.img
cat /dev/block/platform/msm_sdcc.1/by-name/persist  > /storage/sdcard0/persist.img

# Optionally, backup the customization pack from your Carrier
cat /dev/block/platform/msm_sdcc.1/by-name/custpack > /storage/sdcard0/custpack.img
```

## Backup Download

Now you need to get out of the session (`exit`) and go back to your USB-connected
computer. On the computer, download the generated images now via:

```bash
mkdir userbackup;
cd userbackup;

adb pull /storage/sdcard0/boot.img;
adb pull /storage/sdcard0/firmware.img;
adb pull /storage/sdcard0/system.img;
adb pull /storage/sdcard0/persist.img;

# Optionally, backup the customization pack from your Carrier
adb pull /storage/sdcard0/custpack.img;
```


## UNBRICK HOWTO

Please calm down. Make a cup of tea and calm down. You can fix it, it's pretty easy.
This repository contains backup images of all the necessary OEM partitions.

### Kernel / Boot Partition

```bash
adb reboot bootloader
fastboot flash boot backup/boot.img
```

### Firmware / Modem Partition

```bash
adb reboot bootloader
fastboot flash modem backup/firmware.img
```

