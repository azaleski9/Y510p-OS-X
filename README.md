# Mac OS X Mojave/Catalina DP5 on Lenovo Y510P

* This guide is intended to help Lenovo Y510p users to try OS X for educational purposes. You should buy Apple computer to be able to use Apple software legally in compliance with Apple's macOS EULA.
* There can be many small things that doesn't work and I don't know about it. Please make appropriate issues here so we can try fix it together.
* My config assumes you replaced WiFi card with Broadcom BCM94352HMB
* This config was tested on Lenovo Y510P (Intel Core i7, Full HD screen, One Nvidia card)

## Disclaimer
Although no one ever reported broken Y510p because of this guide, I still hold no responsibility for broken systems. Proceed at your own risk,
read, read again, and ask before attempting something you are not OK with. Whoever is going to use part or all of this guide, please backup
everything you are going to tinker with first.

## What doesn't work?

* Discrete Nvidia GPU. If you have 2 Nvidia Card in SLI mode you should remove one from Ultrabay. Nvidia GPU won't never work as Mac OS does not support Optimus. Graphics in SLI mode does not work too. I tried to make some fixes, but I didn't succeded.
* Intel GPU Restart issue. There is a problem related to graphics occurs when display goes off then on, like when system sleeps and wake or when changing display resolution for example, which results in no display on restart. These events requires re-initialization for the graphic driver but it seems the driver re-initialize incorrectly. As a result, the graphics related memory hold wrong data for some graphic registers which indirectly affects the restart functionality. Once the system is restarted in any of these scenarios, the POST (where Lenovo logo should appear) has no display and so is Clover and beyond if an OS is selected and booted using keyboard blindly.
* VGA Port - macOS does not support VGA port. You should use HDMI->VGA adapter instead

## Installation

You should prepare USB drive with Vanilia (untouched) macOS installer. You can read about preparing USB drive with installer and Clover here: https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/building-the-usb-installer

All needed Clover configuration I provide you here. We use the same config for installation and normal use.

## Post-installation

All needed patches are included in Clover directories. You don't need to care about patching system after installation.

## How to configure Clover bootloader?

TLDR. For lazy people (or people who want to verify their config) I attach ready Clover directory, but It may have outdated kexts and patches. You need to fill SMBIOS details (serial number etc.) using for example Clover Configurator. **I always recommend doing own Clover configuration. Why? Because you will encouter many more problems with OS X later. Configuring macOS manually will teach you how everything works** So let's do this.

### 1. Clover installation

You should tick the following drivers while installing bootloader

* ApfsDriverLoader
* AppleKeyFeeder
* AppleUiSupport
* OsxAptioFix3Drv
* PartitionDxe
* Ps2MouseDxe
* UsbKbDxe
* UsbMouseDxe
* VBoxHfs

### 2. config.plist

Just replace this file with that from my repo. Then open that file using Clover Configuration and go to SMBIOS section. You should fill serial number and Board-ID with yours valid values. I can't show you how to do that. All other values leave untouched.

### 3. SSDT patches

Just replace ACPI/patches directory in your Clover Installation with mine.

### 4. Kexts

You should collect these kexts:

* AirportBrcmFixup (Broadcom BCM94352HMB)
* AppleALC (Sound)
* AtherosE2200Ethernet (Etherner adapter)
* BrcmBluetoothInjector, BrcmFirmwareData, BrcmPatchRAM2 (Bluetooth)
* CodecCommander (Sound fix after sleep)
* Lilu
* VirtualSMC, SMCBatteryManager, SMCProcessor, SMCSuperIO
* WhateverGreen (Intel HD)

These kexts I included in my repo and you should push them too:
* USBPorts (Fixes USB ports visible as internal instead external)
* VoodooPS2Controller (I modified some config.plist here, so many OS X gestrues work  including Force Touch emulation. If you want to use later version from original repo just replace config.plist from my kexts or use default config provided by developer)

Push all these kexts to kexts/Other.

That's all with that config Catalina installer should boot. If something does not work compare it with my configuration.

## Links

* Clover bootloader: https://sourceforge.net/projects/cloverefiboot
* Clover configurator: https://mackie100projects.altervista.org/download-clover-configurator/
* AirportBrcmFixup: https://github.com/acidanthera/AirportBrcmFixup
* AppleALC: https://github.com/acidanthera/AppleALC
* AtherosE2200Ethernet: https://github.com/Mieze/AtherosE2200Ethernet
* BrcmPatchRAM: https://github.com/RehabMan/OS-X-BrcmPatchRAM (Mojave) or https://www.insanelymac.com/forum/topic/339175-brcmpatchram2-for-1015-catalina-broadcom-bluetooth-firmware-upload/ (Catalina)
* CodecCommander: https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/
* Lilu: https://github.com/acidanthera/Lilu
* VirtualSMC: https://github.com/acidanthera/VirtualSMC
* WhateverGreen: https://github.com/acidanthera/WhateverGreen
* Original VoodooPS2 sources: https://github.com/acidanthera/VoodooPS2

## Thanks to

* @acidanthera for almost all kexts I use there
* @apianti, @blackosx, @blusseau, @dmazar, @slice2009 for Clover Bootloader
* @Mieze for AtherosE2200Ethernet
* @RehabMan for CodecCommander and BrcmPatchRam
* @mackie100projects for Clover configurator
* @headkaze for fixes to BrcmPatchRAM on 10.15
* @ahmed_ais for first tutorial how to install macOS on Lenovo Y510P
* And many more people about I didn't know.
