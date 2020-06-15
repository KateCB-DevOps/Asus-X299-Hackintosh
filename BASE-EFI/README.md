The BASE-EFI folder contains an EFI based on the OpenCore Vanilla Desktop Guide that should be valid for all ASUS X299 Motherboards.  Refer to the [OpenCore Vanilla Desktop Guide](https://dortania.github.io/OpenCore-Desktop-Guide/) and the [Skylake-X](https://dortania.github.io/OpenCore-Desktop-Guide/config-HEDT/skylake-x.html) for more information.

Important Details:
1. There are two config.plist.  One using iMacPro1,1 SMBIOS and one using MacPro7,1 SMBIOS.
  * If using iMacPro1,1 SMBIOS, you can delete MacProMemoryNotificationDisabler.kext under `EFI-OC-Kexts`
2. OpenCanary GUI already enabled.
3. Assumes that you already have MSR lock disabled in BIOS.  Refer to section [Important BIOS Settings](https://github.com/shinoki7/Asus-X299-Hackintosh#important-bios-settings)
  * To patch BIOS, Refer to section [Patching ASUS BIOS](https://github.com/shinoki7/Asus-X299-Hackintosh#patching-asus-bios-required-on-latest-bios-and-cascade-lake-x-refresh-motherboards)
  * If on patched BIOS or Cascade Lake-X Refresh motherboard, copy [SSDT-AWAC](https://github.com/shinoki7/Asus-X299-Hackintosh/blob/master/SSDT/SSDT-AWAC.aml) to the EFI folder under `EFI-OC-ACPI` and add SSDT-AWAC as an entry in your config.plist under `ACPI-Add`
4.  For ethernet, IntelMausi is enabled.
  * For WS X299 Sage/10G users replace IntelMausi with [SmallTreeIntel8259x.kext](https://github.com/shinoki7/Asus-X299-Hackintosh/blob/master/Kexts/SmallTreeIntel8259x.kext.zip)  and update the kext entry.  NOTE: This already assumes you have patched your EEPROM using Ubuntu.
  * For users with I211 NICs like the X299 Deluxe, copy the SmallTreeIntel92576 kext to your EFI folder and add a new kext entry under `Kernel-Add`
5.  XhciPortLimit is enabled in the config.plist under `Kernel-Quirks` with USBInjectAll.Kext
  * Once your computer is up and running, it is highly recommended to create your own USB kext instead. Please use [this](https://dortania.github.io/USB-Map-Guide/) as a proper guide to map your USB ports.
6.  You will need to create your own Serial Number and SMUUID.  Instructions can be found [here](https://dortania.github.io/OpenCore-Desktop-Guide/config-HEDT/skylake-x.html#platforminfo)
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
