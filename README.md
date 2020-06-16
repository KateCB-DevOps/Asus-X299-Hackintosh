# Introduction
The ASUS X299 Hackintosh repo contains OpenCore EFI distributions and related files that can be used as a reference when starting or migrating your X299 Hackintosh to OpenCore.  While the EFIs can be used as a starting point and should be compatible with all ASUS X299 boards, it is still highly recommended to review [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/) and [Skylake-X section](https://dortania.github.io/OpenCore-Desktop-Guide/config-HEDT/skylake-x.html) for more information.

References: 
* [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/)
* [OpenCore Documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

# Folders
* ASUS BIOS Patch - Contains version of UEFITool to patch ASUS BIOS versions (3006, 3101) and BIOS 0603 for Cascade Lake-X Refresh Motherboards as the CFG lock option in the BIOS is broken.  This patch disables the lock so we don't have to enable `AppleCpuPMCfgLock` and `AppleXcpmCfgLock`.  Instructions to apply patch are in section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards)
* BASE-EFI - OpenCore EFIs with the OpenCanary GUI that should be valid for all ASUS X299 boards.  Refer to section [BASE-EFI Configuration](https://github.com/shinoki7/Asus-X299-Hackintosh#base-efi-configuration) for more details.
* EFI-Validated-Distributions (Archive) - Validated EFIs from other users (Please use this as a reference only as these are not updated)
* XHC-USB-Kexts - USB kexts created by users for specific motherboards.  Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports.

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

# BASE EFI Configuration
1. Download the latest EFI zip in the Base-EFI folder.
    * Download [Resources](https://github.com/acidanthera/OcBinaryData/tree/master/Resources) folder and copy to the EFI folder `EFI-OC`
2. Review
    * Important BIOS Settings
        * Above 4G Encoding: Enabled
        * MSR Lock: Disabled
            * If option isn't available in BIOS, turn on `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` in config.plist under Kernel-Quirks.
            * If on patched ASUS BIOS, MSR lock will already be disabled.
        * CSM: Disabled
    * There are two config.plist:  One using iMacPro1,1 SMBIOS and one using MacPro7,1 SMBIOS. Depending on which SMBIOS you choose, rename the file to config.plist and delete the other one.  
        * MacPro7,1 SMBIOS is only compatible with macOS Catalina and higher
        * If using iMacPro1,1 SMBIOS, you can delete MacProMemoryNotificationDisabler.kext under `EFI-OC-Kexts` 
    * Assumes that you already have MSR lock disabled in BIOS.
        * To patch BIOS, Refer to section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards)
        * If on patched BIOS or Cascade Lake-X Refresh motherboard, copy [SSDT-AWAC](https://github.com/shinoki7/Asus-X299-Hackintosh/blob/master/SSDT/SSDT-AWAC.aml) to the EFI folder under `EFI-OC-ACPI` and add SSDT-AWAC as an entry in your config.plist under `ACPI-Add`
3. Configuration
    * Ethernet: 
        * For WS X299 Sage/10G users replace IntelMausi with [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) kext and update the kext entry.  NOTE: Requires Ubuntu EEPROM modding outlined in @KGPs [guide section E.8.2.2](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/)
        * For users with I211 NICs like the X299 Deluxe, copy the [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases) kext to your EFI folder and add a new kext entry under `Kernel-Add`
    * TSCAdjustReset:
        * From the Kexts-TSCAdjustReset [folder](https://github.com/shinoki7/Asus-X299-Hackintosh/tree/master/Kexts/TSCAdjustReset), download the zip with the number of cores your processor and extract the file.  Copy this file to your EFI folder under `EFI-OC-Kexts`
    * PlatformInfo: 
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
4. Post-Install
    * It is highly recommended to create your own USB kext. Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports.

# Additional Kexts
* [SmallTreeIntel8259x](https://small-tree.com/support/downloads/10-gigabit-ethernet-driver-download-page/) 
  * Enables built-in Intel 10G ethernet ports on the Sage/10G.
* [IntelMausi](https://github.com/acidanthera/IntelMausi/releases)
  * Enables ethernet for most intel controllers
* [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)
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
