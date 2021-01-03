# How to install Ubuntu 14.04/16.04 on Alienware 15 R2 & Alienware Aurora R8

Tiny guide to install Ubuntu 14.04 & Ubuntu 16.04 on a brand new Alienware 15 R2 & Alienware Aurora R8

## Let windows 10 install
Just next, next, next filling up your data.

You should get a BIOS update alert from the Alienware Update widget. If not,
right click on the Down arrow icon in the bottom right extra icons `^` thing and 
right click, then click `Check for Updates`.

Install it. It will reboot your computer, try to not touch anything up until
you are back to Windows.

## Shrink the disk
Go to the disk manager (right click on Windows icon > Disk management) and
Shrink the OS partition (right click on it, `Shrink Volume...`). It offered
me shrinking by 115XXX MB. Just shrink it 110000 MB. Shrinking it more won't work.

## Get Windows to boot on AHCI mode (SATA options)
For Ubuntu to see the NVME disk it needs to boot on AHCI not on RAID mode (the default).
As you don't want to go to the BIOS to change the SATA mode every time you want to boot
in one or another OS, we need to force Windows to be able to boot in AHCI mode.

For that follow the instructions (kindly taken from [here](http://www.tenforums.com/drivers-hardware/15006-attn-ssd-owners-enabling-ahci-mode-after-windows-10-installation.html)):

1. Run Command Prompt as Admin.
2. Invoke a Safe Mode boot with the command: `bcdedit /set {current} safeboot minimal`.
3. Restart the PC and enter your BIOS during bootup (F2 key).
4. In tab `Advanced` change option `SATA Operation` from `RAID on` to `AHCI` mode then go to `Exit` tab and use `Save Changes and Reset`.
5. Windows 10 will launch in Safe Mode.
6. Right click the Window icon and select to run the Command Prompt in Admin mode from among the various options.
7. Cancel Safe Mode booting with the command: `bcdedit /deletevalue {current} safeboot`.
8. Restart your PC once more and this time it will boot up normally but with AHCI mode activated.
9. Enjoy your awesomeness.


## Disable Secure boot
You need to disable secure boot in order to boot any other OS.

Enter your BIOS (F2 key on boot). Go to `Boot` tab and change `Secure Boot` option to `Disabled`.

Note that `Boot List Option` should be `UEFI` (it's the default).

## Get the latest Ubuntu 14.04 or Ubuntu 16.04 and write it to a bootable pendrive

- [Ubuntu 14.04](https://releases.ubuntu.com/14.04/): Select the 64-bit PC (AMD64) desktop image
- [Ubuntu 16.04](https://releases.ubuntu.com/16.04/): Select the 64-bit PC (AMD64) desktop image

Using [Universal USB Installer](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/) for writing my images, format the pendrive to FAT32.

## Download Nvidia Driver (Only for Nvidia GPU)

If you are using a Nvidia GPU, download the corresponding [Nvidia diver](https://www.nvidia.com/Download/index.aspx) and place it into the USB driver above.

## Boot from the live linux pendrive
Press F12 while booting and choose under `UEFI OPTIONS` to boot from your pendrive, for me it was
`USB1 - UEFI OS( USB DISK 3.0 PMAP)`.

## Use the install Wizard
Just select the `Install Ubuntu` and disconnect the Internet connection.

Configure as you like BUT DON'T ENABLE DOWNLOAD UPDATES WHILE INSTALLING NOR INSTALL THIRD PARTY SOFTWARE. It will freeze your installation. If you don't believe me, just try and enjoy your reboot.

Click on `Something else`.

Now you should see some partitions like `/dev/nvme0n1`. If you don't, you missed some step.

Now choose the free space partition that corresponds to the shrinked space we made before. For me it's 115343 MB. I'll just make a partition for `/` and another `swap` one.

In order to be able to hibernate in Ubuntu you'll need at least your amount of RAM as swap. I doubt very much it will actually work, but hey, you need to try.

I have 16GB of RAM so I'll do 115343 - 17 * 1024 = 97884 MB partition. (Yeah that's a 17, I'm a bit lazy to check for how much exactly it should be).

Click on that `free space` to be selected and click on the `+` symbol. Put your amount of MB for it (97884) in Size. Choose `Logical` as Type. Leave Location as `Beginning of this space`. Use as `Ext4`. Mount point as `/`.

Then on the left free space, repeat the process but make it of type `swap`.

**IMPORTANT** now you need to change the `Device for boot loader installation` to `/dev/nvme0n1`.

Now you can click `Install Now`.

In a few minutes you should be good to go!


