# ASUS

EFI-Folder distributions separated by motherboard for validated working systems.  

The BASE EFI can be used as a starting point that should be valid for ASUS x299 motherboards.

It is recommended to add device properties for your specific motherboard.
I.E: Sage/10G users use the WS X299 Sage 10G folder and copy the device properties from 'DJ7_ASUS_WS_SAGE10G_Properties.zip' to your OpenCore EFI folder and config.plist

NOTE:
- Remember to adapt the TSCAdjustReset.kext IOCPUNumber to your amount of threads - 1. (I.E 18-core i9-7980XE would result in 35 (36 threads -1)
- Populate Serial Number, UUID, ROM, MLB
- For ROG users (Rampage VI Extreme, Rampage VI Extreme Omega, x299 Strix, Rampage VI Apex), remember to adjust the layout-id and model under Device Properties (PciRoot(0x0)/Pci(0x1f,0x3))

CREDITS:
izo1 and shael
