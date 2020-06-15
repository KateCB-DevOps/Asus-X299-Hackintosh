# Introduction
The ASUS X299 Hackintosh repo contains OpenCore EFI distributions and related files that can be used as a reference when starting or migrating your X299 Hackintosh to OpenCore. 

References: 
* [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/)
* [OpenCore Documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

# Folders
* ASUS BIOS Patch - Contains version of UEFITool to patch ASUS BIOS versions (3006, 3101) and BIOS 0603 for Cascade Lake-X Refresh Motherboards as the CFG lock option in the BIOS is broken.  This patch disables the lock so we don't have to enable `AppleCpuPMCfgLock` and `AppleXcpmCfgLock`.  Instructions to apply patch are in section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards)
* BASE-EFI - OpenCore EFI with the OpenCanary GUI built on latest release 0.5.9 that should be valid for all ASUS X299 boards.  Refer to README in BASE-EFI folder for more information.
* EFI-Validated-Distributions (Archive) - Validated EFIs from other users (Please use this as a reference only as these are not updated)
* XHC-USB-Kexts - USB kexts created by users for specific motherboards.  Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports.

# Important BIOS Settings
* Above 4G Encoding: Enabled
* MSR Lock: Disabled
  * If option isn't available, turn on `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` in config.plist under Kernel-Quirks.
  * If on patched ASUS BIOS, MSR lock will already be disabled.
* CSM: Disabled

# What Works
* Sleep / Wake
* Wifi and Bluetooth (Using natively supported Broadcom BCM94360CD)
* Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
* iMessage, FaceTime, App Store, iTunes Store
* Ethernet
* 10G Ethernet
* HEVC, H.264
* Onboard audio
* TRIM
* USB 2.0 / USB 3.0
* USB 3.1 Gen 2
* Thunderbolt 3 w/ hot plug including Thunderbolt Local Node and Thunderbolt Bus. Credits @CaseySJ and contributors in his [thread](https://www.tonymacx86.com/threads/success-gigabyte-designare-z390-thunderbolt-3-i7-9700k-amd-rx-580.267551/)
* DRM
* Native NVRAM
* CPU Power Management
* USB Power

# What Doesn't Work
* SideCar due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS (Using Duet Display as alternative)

# Required SSDTs
* SSDT-EC-USBX.aml
* SSDT-PLUG.aml
* SSDT-SBUS-MCHC.aml

# Required Kexts
* AppleALC.kext
* Lilu.kext
* VirtualSMC.kext
  * SMCProcessor.kext
  * SMCSuperIO.kext
* WhateverGreen.kext
* TSCAdjustReset.kext
  * Located in Kexts-TSCAdjustReset folder and adjusted for cores.  Remove the '-(CoreCount)' when copying to your EFI folder.
* MacProMemoryNotificationDisabler.kext
  * Only needed for MacPro7,1 to disable the Memory module error notification

# Additional SSDTs
* SSDT-AWAC.aml
  * Required on Cascade-Lake X Refresh motherboards and latest ASUS BIOS

# Additional Kexts
* SmallTreeIntel8259x 
  * Enables built-in Intel 10G ethernet ports on the Sage/10G.  Requires Ubuntu EEPROM modding outlined in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
* IntelMausi
  * Enables ethernet for most intel controllers
* SmallTreeIntel82576
  * Enables ethernet for I211 NICs
  * Version 1.3 is named (SmallTreeIntel82576-Catalina.kext), for older OS use (SmallTreeIntel8259x.kext)
* [AGPMInjector.kext](https://github.com/Pavo-IM/AGPMInjector) 
  * Apple Graphics Power Management injector

# Patching ASUS BIOS (Required on latest BIOS and Cascade Lake-X Refresh Motherboards)
In the latest release of ASUS BIOS for X299 Motherboards (BIOS 3006, 3101) and Cascade Lake-X Refresh boards, the MSR lock option is broken so we will need to patch it in order disable it.  
NOTE: Your motherboard needs to support BIOS FlashBack (Refer to your motherboard's manual)

1.  Download UEFIPatch in the ASUS Bios Patch folder and copy latest version of your BIOS in same folder
2. Run UEFIPatch by running terminal and changing your directory to where you copied UEFIPatch. For example: 'cd /Users/user123/Downloads/UEFIPatch' (Replace 'user123' with your user name)
3. Then enter: './UEFIPatch WS-X299-SAGE-10G-ASUS-3101.CAP' (Replace the file name of bios with whatever you named your bios)
4. You should see some lines outputted in terminal ending with 'Image patched' and a new .CAP file with a .patched extension.  Refer to your motherboard's manual (Search for BIOS FlashBack) and rename the .patched file you just created. (For example, WS X299 Sage/10G users, rename the .patched file to 'WSXTG.CAP')
5. Perform BIOS Flashback.
6. Add SSDT-AWAC.aml from the SSDT Folder to your EFI.
