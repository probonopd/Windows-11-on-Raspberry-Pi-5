# Windows 11 on Raspberry Pi 5

Information on how to install Windows 11 IoT Enterprise LTSC 2024 on the Raspberry Pi 5 is hard to find or is not even available. Through trial and error I find the following works up to a certain point, at which I don't know how to proceed further. Any hints appreciated (in GitHub Issues).

* Download `en-us_windows_11_iot_enterprise_ltsc_2024_arm64_dvd_ec517836.iso`
* Download and install RUFUS
* Download UEFI for Raspberry Pi 5 from https://github.com/worproject/rpi5-uefi/releases
* Download `WinEFIMounter-static.exe`
* Install the Windows ISO to a USB stick using RUFUS
* Copy the 3 files from rpi5-uefi to the root directory of the FAT32 partition on the USB stick
* Boot the Raspberry Pi 5 from the USB stick while a microSD card is inserted
* It should boot into the Windows 11 installer
* Delete all partitions on the microSD card and install Windows 11 into the Unallocated Space
* At the end, there will be a dialog box saying "Windows 11 installation failed" without further explanation
* At that point, shut down and power off the Raspberry Pi 5
* The microSD card is currently unbootable, because it lacks the rpi5-uefi files and the Windows 11 EFI boot files and BCE files
* We need to put those on the 100 MB ESP partition on the microSD card; unfortunately Windows 11 doesn't let one mount these easily. Hence we use use `WinEFIMounter-static.exe`
* Unfortunately the Explorer in Windows 11 doesn't let one browse that partition easily either. Hence we have to use the command line with Administrative Privileges and run commands like `xcopy D:\$Windows.~BT\Efi\*.* Z:\ /s /e /h` (assuming that the USB stick with the Windows installer is `D:` and the mounted ESP is `Z:`)
* Similarly, copy the 3 rpi5-uefi files
* Booting the microSD card now brings up the Windows bootloader, saying no valid BCE was found
* Boot the Raspberry Pi 5 from the USB stick
* Press Shift + F10
* __FIXME:__ How to use command-line tools like `bootrec` or `bcdboot` to repair the boot configuration or recreate the BCD store?
* __FIXME:__ At this point, Windows 11 seems to be "half-installed" in `$WINDOWS.~BT\NewOS\Windows`
