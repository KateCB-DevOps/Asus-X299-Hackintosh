# Introduction
The ASUS X299 Hackintosh repo contains OpenCore EFI distributions and related files that can be used as a reference when starting your X299 Hackintosh. It is currently based on the latest release 0.5.8

Please refer to the [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/) for a proper guide.  
You can use the Base-EFI folder as a reference. 

Additional references: [OpenCore Documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

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
* SideCar (Using Duet Display as alternative)

# Required SSDTs
* SSDT-EC-USBX.aml
* SSDT-PLUG.aml
* SSDT-SBUS-MCHC-DTGP.aml

# Required Kexts
* AppleALC.kext
* Lilu.kext
* VirtualSMC.kext (Include SMCProcessor.kext, SMCSuperIO.kext)
* WhateverGreen.kext
* TSCAdjustReset.kext (Right-click the kext and click Show Package Contents.  Adjust IOCPUNumber in Info.plist-> IOKitPersonalities-> TSCAdjustReset-> IOPropertyMatch) to number of threads - 1.  For example, i9-7980XE would be 35)
* MacProMemoryNotificationDisabler.kext (Only needed for MacPro7,1 to disable the Memory module error notification)

# Additional SSDTs
* SSDT-AWAC.aml (Required on Cascade-Lake X Refresh motherboards and latest ASUS BIOS)

# Additional Kexts
* SmallTreeIntel8259x.kext (Enables built-in 10G ethernet ports on the Sage/10G.  Requires Ubuntu EEPROM modding outlined in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
* IntelMausiEthernet.kext (Enables ethernet for most intel controllers)
* X299USB.kext (Maps USB ports.  Please use this as a reference only and follow the guide to create your own)
* [AGPMInjector.kext](https://github.com/Pavo-IM/AGPMInjector) (Apple Graphics Power Management injector)

# Patching ASUS BIOS (Required on latest BIOS and Cascade Lake-X Refresh Motherboards)
In the latest release of ASUS BIOS for X299 Motherboards and Cascade Lake-X Refresh boards, the MSR lock option is broken so we will need to patch it in order disable the MSR lock.  
NOTE: Your motherboard needs to support BIOS FlashBack (Refer to your motherboard's manual)

1.  Download UEFIPatch in the ASUS Bios Patch folder and copy latest version of your BIOS in same folder
2. Run UEFIPatch by running terminal and changing your directory to where you copied UEFIPatch. For example: 'cd /Users/user123/Downloads/UEFIPatch' (Replace 'user123' with your user name)
3. Then enter: './UEFIPatch WS-X299-SAGE-10G-ASUS-3101.CAP' (Replace the file name of bios with whatever you named your bios)
4. You should see some lines outputted in terminal ending with 'Image patched' and a new .CAP file with a .patched extension.  Refer to your motherboard's manual (Search for BIOS FlashBack) and rename the .patched file you just created. (For example, WS X299 Sage/10G users, rename the .patched file to 'WSXTG.CAP')
5. Perform BIOS Flashback.
