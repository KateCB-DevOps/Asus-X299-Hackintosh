# Introduction
The ASUS X299 Hackintosh repo contains OpenCore EFI distributions and related files that can be used as a reference when starting or migrating your X299 Hackintosh to OpenCore. 

References: 
* [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/)
* [OpenCore Documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

# Folders
* ASUS BIOS Patch - Contains version of UEFITool to patch ASUS BIOS versions (3006, 3101) and BIOS 0603 for Cascade Lake-X Refresh Motherboards as the CFG lock option in the BIOS is broken.  This patch disables the lock so we don't have to enable `AppleCpuPMCfgLock` and `AppleXcpmCfgLock`.  Instructions to apply patch are in section (Patching ASUS BIOS)
* Base-EFI - OpenCore EFI with the OpenCanary GUI built on latest release 0.5.8 that should be valid for all ASUS X299 boards.  However, please use this EFI as a reference and refer to the Vanilla Desktop Guide for a proper guide. 
* EFI-Validated-Distributions - Validated EFIs from other users
* Kexts - Optional kexts that are optional depending on your build.
* SSDT - Optional SSDTs that are optional depending on your build.

![About This Mac](https://i.imgur.com/0Tz7n3S.png)
![PCI](https://i.imgur.com/XMO19u3.png)
![Thunderbolt](https://i.imgur.com/sVvm1qK.png)
# Hardware Specifications
* Motherboard: ASUS WS X299 Sage/10G; BIOS 3101
* CPU Cooler: Fractal Design Celsius+ S36
* CPU: Intel i9-9960X
* RAM: 4x16 Corsair Vengeance RGB 3200 @2933
* SSD: Samsung 970 EVO 1 TB
* GPU: Sapphire Pulse RX 580
* Wifi/BT: Broadcom BCM94360CD
* PSU: Corsair RM850X
* Case: Lian Li PC-011 Dynamic

# Important BIOS Settings
* Above 4G Encoding: Enabled
* MSR Lock: Disabled
  * If option isn't available, turn on `AppleCpuPmCfgLock` and `AppleXcpmCfgLock`.
  * If on patched ASUS BIOS, MSR lock will already be disabled.
* CSM: Disabled
* OS Type: Windows UEFI

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
* SSDT-EC-USBX
* SSDT-PLUG
* SSDT-SBUS-MCHC

# Required Kexts
* AppleALC
* Lilu
* VirtualSMC
  * SMCProcessor
  * SMCSuperIO
* WhateverGreen
* TSCAdjustReset
  * Adjust IOCPUNumber in `Info.plist-> IOKitPersonalities-> TSCAdjustReset-> IOPropertyMatch` to number of threads - 1.  For example, i9-7980XE would be 35
* MacProMemoryNotificationDisabler
  * Only needed for MacPro7,1 to disable the Memory module error notification

# Additional SSDTs
* SSDT-AWAC 
  * Required on Cascade-Lake X Refresh motherboards and latest ASUS BIOS

# Additional Kexts
* SmallTreeIntel8259x 
  * Enables built-in Intel 10G ethernet ports on the Sage/10G.  Requires Ubuntu EEPROM modding outlined in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
* IntelMausiEthernet
  * Enables ethernet for most intel controllers
* X299USB 
  * Maps USB ports.  Please use this as a reference only and follow the [guide](https://dortania.github.io/USB-Map-Guide/) to create your own
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
