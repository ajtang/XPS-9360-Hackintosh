# XPS-9360-Hackintosh

This repository contains a sample configuration to run macOS (Currently Catalina `10.15.3`) on a Dell XPS 9360
Booting is enabled using  OpenCore.

## Hardware Configuration

- Dell XPS 9360
  - Intel i7-8550U
  - 16GB RAM
  - Sharp `SHP144` `LQ133Z1` QHD+ (3200x1800) Touchscreen display (VoodooI2C.kext)
  - Samsing 960 EVO  NVME 512GB SSD 
  - Dell DW1560 Wireless (eBay)
    - Wi-Fi device ID [`14e4:43b1`], shows as Apple Airport Extreme due to `FakePCIID_Broadcom_WiFi.kext`
    - Bluetooth device ID [`0a5c:216f`], chipset `20702A3` with firmware `v14 c5882` using `BrcmPatchRAM3.kext`
  - Sonix Technology Webcam, device ID [`0c45:670c`], works out of the box
  - Validity Inc. Finger print scanner, device ID [`138a:0091`], [linux open-source project](https://github.com/hmaarrfk/Validity91)
  - Disabled devices
    - SD card reader, [macOS open-source project](https://github.com/sinetek/Sinetek-rtsx)

- Firmware Revisions
  - BIOS version `2.9.0`
  - Thunderbolt Controller firmware version `NVM 26`

## PREP

## UEFI Variables

EFI BIOS variables need to be modified in order for proper funcitonality.
The following variables need to be updated:

| Variable              | Offset | Default value  | Desired value   | Comment                                                    |
|-----------------------|--------|----------------|-----------------|------------------------------------------------------------|
| CFG Lock              | 0x4de  | 0x01 (Enabled) | 0x00 (Disabled) | Disable CFG Lock to prevent MSR 0x02 errors on boot        |
| DVMT Pre-allocation   | 0x785  | 0x01 (32M)     | 0x06 (192M)     | Increase DVMT pre-allocated size to 192M for QHD+ displays |
| DVMT Total Gfx Memory | 0x786  | 0x01 (128M)    | 0x03 (MAX)      | Increase total gfx memory limit to maximum                 |

## OpenCore Configuration

All OpenCore ACPI hotpatches are included in aml format.

## CPU Profile

macOS needs a way to effectively manage the power profile of the i7-8550U processor, it is necessary to include a powermanagement profile for `X86PlatformPlugin`.

A pre-built `CPUFriend.kext` and `CPUDataProvider.kext` is included in the `kext` folder for the i7-8550U.

## Undervolting

The undervolt settings used are configured with the following settings:

- Overclock, CFG, WDT & XTU enable  
  `0x4DE` -> `00`  
  `0x64D` -> `01`  
  `0x64E` -> `01`

- Undervolt values:  
  `0x653` -> `0x64` (CPU: -100 mV)  
  `0x655` -> `01`   (Negative voltage for `0x653`)  
  `0x85A` -> `0x1E` (GPU: -30 mV)  
  `0x85C` -> `01`   (Negative voltage for `0x85A`)


## Credits

- [OS-X-Clover-Laptop-Config (Hot-patching)](https://github.com/RehabMan/OS-X-Clover-Laptop-Config)
- [Dell XPS 13 9360 Guide by darkvoid](https://github.com/the-darkvoid/XPS9360-macOS)
- [VoodooI2C on XPS 13 9630 by Vygr10565](https://www.tonymacx86.com/threads/guide-dell-xps-13-9360-on-macos-sierra-10-12-x-lts-long-term-support-guide.213141/page-202#post-1708487)
