# ASUS X299 Hackintosh

# Introduction
The ASUS X299 Hackintosh repo contains OpenCore EFI distributions and related files that can be used as a reference when starting or migrating your X299 Hackintosh to OpenCore.  While the EFIs can be used as a starting point and should be compatible with all ASUS X299 boards, it is still highly recommended to review the [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/) and [Skylake-X section](https://dortania.github.io/OpenCore-Desktop-Guide/config-HEDT/skylake-x.html) for a proper guide.

# Folders
| Folder | Description |
| :------------- | :---------- |
| ASUS BIOS Patch | Contains version of UEFITool to patch ASUS BIOS versions (3006, 3101) and BIOS 0603 for Cascade Lake-X Refresh Motherboards as the CFG lock option in the BIOS is broken.  This patch disables the lock so we don't have to enable `AppleCpuPMCfgLock` and `AppleXcpmCfgLock`.  Instructions to apply patch are in section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards) |
| Custom BIOS Collection | Contains modified BIOS files that have custom splash boot logos |
| BASE-EFI | OpenCore EFIs with the OpenCanary GUI that should be valid for all ASUS X299 boards.  Refer to section [BASE-EFI Configuration](https://github.com/shinoki7/Asus-X299-Hackintosh#base-efi-configuration) for more details. |
| EFI-Validated-Distributions (Archive) | Validated EFIs from other users (Please use this as a reference only as these are not updated) | 
| XHC USB Kexts | USB kexts created by users for specific motherboards.  Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports. |

# Personal Build Specifications
## Components
* Motherboard : ASUS WS X299 Sage/10G
    * BIOS 3101
* Processor : Intel i9 9960X
* CPU Cooler : Fractal Design S36+ AIO
* RAM : 4x16 Corsair Vengeance RGB Pro 3200 Mhz
    * Currently clocked at 2933 Mhz
* Boot Drive : Samsung 970 EVO 1 TB
* Graphics Card : Sapphire RX 580 Pulse 8 GB
* Power Supply : Corsair RM 850x
* Case : Lian Li PC 011 Dynamic

## Additional Components / Peripherals
* 2X Gigabyte Titan Ridge Thunderbolt 3 Card 
    * Flashed with custom NVM50 firmware
* Apple Magic Keyboard 2 (Space Gray)
* Apple Magic Trackpad 2 (Space Gray)
* Apple Magic Mouse 2 (Space Gray)
* LG 27UL600 27" 4K UHD Monitor
* LG 27UK600 27" 4K UHD Monitor
* LG C9 65" 4K UHD TV

## What Works
* Sleep / Wake
* Wifi and Bluetooth (Using natively supported Broadcom BCM94360CD)
* Handoff, Continuity, AirDrop, Continuity Camera, and Unlock with Apple Watch
* iMessage, FaceTime, App Store, iTunes Store
* Ethernet
* 10Gb Ethernet
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

## What Doesn't Work
* SideCar due to some T2 chip dependancies on MacPro7,1 and iMacPro1,1 SMBIOS (Using Duet Display as alternative)

## Comments
The ASUS WS X299 Sage series (WS X299 Sage, WS X299 Sage/10G, Pro WS X299 Sage II) are great motherboards with 7 PCIe slots running at 16x/8x/8x/8x/8x/8x/8x and multiple M.2/U.2 connections.  The Sage/10G even includes dual 10Gb Intel X550-AT2 LAN ports that are compatible with macOS. Unfortunately the motherboards only have a few USB ports and only a single 19 Pin USB 3.0 header for internal ports.  In order to connect internal USB devices such as Bluetooth or RGB Controllers there are a few options.  Note that the specific cables/card listed below are examples.  Just make sure the PCIe card is compatible with macOS.
* 1. [USB 3.0 20 Pin Female to USB 2.0 Pin Male Adapter](https://www.amazon.com/gp/product/B01MFB04JP/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * With this you will not have a USB 3.0 header to connect to the front of your case
    * Currently using this in my build with a [USB 2.0 9 Pin Header 1 to 4 Extension Hub Splitter](https://www.amazon.com/gp/product/B085KVH16T/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1) to connect Bluetooth and the USB 2.0 cables for my 2 thunderbolt 3 cards.
* 2. [USB 2.0 IDC 5 Male to USB A Male adapter](https://www.amazon.com/gp/product/B000V6WD8A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Uses one of the USB ports on the back of the motherboard to connect internal devices in the case.  Can use two of these plus regular USB A extension cables if wire isn't long enough or your USB device has 9 pins.
* 3. PCIe USB 3.0 Card with Internal USB Connector
    * [Inateck USB 3.0 Card](https://www.amazon.com/Inateck-Express-Controller-Internal-Connector/dp/B00JFR2H64/ref=sr_1_3?dchild=1&keywords=inateck+pcie+card&qid=1592455853&s=electronics&sr=1-3)
    * [StarTech USB 3.1 PCIe Card](https://www.amazon.com/gp/product/B01I39D15A/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
    * Can use the internal header on the card for the case USB ports or to connect internal devices.
    * NOTE: Wake from Bluetooth devices does not work with this so it's best to connect Bluetooth to one of the motherboard ports.

# Base EFI Configuration
The Base EFI folder contains a prebuilt EFI that should be valid for all ASUS X299 motherboards.  It is currently built using OpenCore 0.5.9 with the OpenCanary GUI enabled following the Dortania OpenCore Vanilla Guide.

## 1. Important BIOS Settings
* Above 4G Encoding: Enabled
* MSR Lock: Disabled
    * If option isn't available in BIOS, turn on `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` in config.plist under Kernel-Quirks.
    * If on patched ASUS BIOS, MSR lock will already be disabled.
        * To patch BIOS, Refer to section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards)
        * If you are not on a patched BIOS or Cascade Lake-X Refresh Motherboard, you can disable the SSDT-AWAC.aml entry in the config.plist under `ACPI-Add`.  You can also delete the file from the EFI folder under `EFI-OC-ACPI`.
* CSM: Disabled

## 2. Configuration
1. Download all the contents in the BASE-EFI folder
    * Download the [Resources](https://github.com/acidanthera/OcBinaryData/tree/master/Resources) folder and copy it to your EFI folder under `EFI-OC`
2. There are two config.plist located in the EFI folder: One using iMacPro1,1 SMBIOS and one using MacPro7,1 SMBIOS. Depending on which SMBIOS you choose, rename the file to config.plist and delete the other one.
    * MacPro7,1 SMBIOS is only compatible with macOS Catalina and higher
    * If using iMacPro1,1 SMBIOS, you can delete MacProMemoryNotificationDisabler.kext under `EFI-OC-Kexts` 
3. Ethernet: 
    * For WS X299 Sage/10G users replace IntelMausi with [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) kext and update the kext entry.  NOTE: Requires Ubuntu EEPROM modding outlined in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
    * For users with I211 NICs like the X299 Deluxe, copy the [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases) kext to your EFI folder and add a new kext entry under `Kernel-Add`
4. TSCAdjustReset:
    * From the Kexts-TSCAdjustReset [folder](https://github.com/shinoki7/Asus-X299-Hackintosh/tree/master/Kexts/TSCAdjustReset), download the zip with the number of cores your processor and extract the file.  Copy this file to your EFI folder under `EFI-OC-Kexts`
5. PlatformInfo: 
    You will need to create your own Serial Number and SMUUID.  Instructions can be found [here](https://dortania.github.io/OpenCore-Desktop-Guide/config-HEDT/skylake-x.html#platforminfo)
    * Remember to adjust the Type depending on which SMBIOS you are using.  Either iMacPro1,1 or MacPro7,1
    * Since Automatic and Generic doesn't populate the processor type in About This Mac, Automatic is set to No and there are additional fields to populate in the config.plist.  Using your results from GenSMBIOS, adjust the following (replace 'Removed!!')
        * `PlatformInfo-DataHub`
            * SystemSerialNumber: Serial
            * SystemUUID: SmUUID
        * `PlatformInfo-Generic`
            * MLB: Board Serial
            * SystemSerialNumber: Serial
            * SystemUUID: SmUUID
        * `PlatformInfo-PlatformNVRAM`
            * MLB: Board Serial
        * `PlatformInfo-SMBIOS`
            * BoardSerialNumber: Board Serial
            * ChassisSerialNumber: Serial
            * SystemSerialNumber: Serial
            * SystemUUID: SmUUID
6. Post-Install
    * It is highly recommended to create your own USB kext. Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports.

# Additional Kexts
* [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) 
  * Enables built-in Intel 10G ethernet ports on the Sage/10G.
  * Install the version compatible with your version of macOS.
  * Ubuntu EEPROM modding in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/) is required for this kext to work.
* [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)
  * Enables ethernet for most intel controllers
* [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)
  * Enables ethernet for I211 NICs
  * Version 1.3 is for macOS Catalina, Version 1.2.5 is for macOS 10.13 and 10.14
* [AGPMInjector](https://github.com/Pavo-IM/AGPMInjector) 
  * Apple Graphics Power Management injector

# Patching ASUS BIOS (Required on latest BIOS and Cascade Lake-X Refresh Motherboards)
In the latest release of ASUS BIOS for X299 Motherboards (BIOS 3006, 3101) and Cascade Lake-X Refresh boards, the MSR lock option is broken so we will need to patch it in order disable it.  
NOTE: Your motherboard needs to support BIOS FlashBack (Refer to your motherboard's manual)

1.  Download UEFIPatch in the ASUS Bios Patch folder and copy latest version of your BIOS in same folder
2. Open Terminal and run the following commands: 
    * 'cd /Users/user123/Downloads/UEFIPatch' 
        * Replace 'user123' with your user name
        * If you extracted the UEFIPatch folder to another directory, change the path in the command above.
    * './UEFIPatch WS-X299-SAGE-10G-ASUS-3101.CAP'
        * Replace the file name of bios with whatever you named your bios
3. You should see some lines outputted in terminal ending with 'Image patched' and a new .CAP file with a .patched extension.  Refer to your motherboard's manual (Search for BIOS FlashBack) and rename the .patched file you just created. (For example, WS X299 Sage/10G users, rename the .patched file to 'WSXTG.CAP')
4. Perform BIOS Flashback.
5. Double check that [SSDT-AWAC.aml](https://github.com/shinoki7/Asus-X299-Hackintosh/blob/master/SSDT/SSDT-AWAC.aml) is in your EFI folder under `EFI-OC-ACPI`.
6. Double check that SSDT-AWAC.aml is an entry in your config.plist under `ACPI-Add`.  If not add a new one with these values
    * Comment (String) SSDT-AWAC.aml
    * Enabled (Boolean) YES
    * Path (String) SSDT-AWAC.aml

# Credits
* Apple : macOS
* [Acidanthera](https://github.com/acidanthera) : OpencorePkg, kexts, etc.
* [Dortania](https://github.com/dortania) : Opencore guide
* [dracoflar](https://github.com/khronokernel) : Modified SSDT-EC-USBX, PLUG, and SBUS-MCHC files, SmallTree 211 patch
* [Pavo](https://github.com/Pavo-IM) : AGPMInjector
* DSM2, Download-Fritz, and vit9696 : SSDT-AWAC and UEFITool for new ASUS BIOS and Cascade Lake-X Refresh motherboard support
* izo1, shael : EFIs and custom boot splashscreen logo
