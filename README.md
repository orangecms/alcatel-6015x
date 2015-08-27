
# Alcatel Onetouch Fire E / Alcatel 6015x


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


## Rooting

The root process requires fastboot and adb.

First, boot the recovery image and **DO NOT FLASH** it:

```bash
sudo fastboot boot custom/recovery.img;
```

- Wait for the boot of the device until you see the Recovery Mod.
- Go to "Install zip" > "Install zip from sideload"

On your computer, sideload the SuperSU binaries now:

```bash
sudo adb sideload custom/SuperSU-v2.46.zip;
```

Wait for the sideload installation process to be completed, then
reboot your phone via `adb reboot`.


## Backup Images

- TODO: Upload OEM FirefoxOS images to this repository

