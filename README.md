# MSI B850M-G Hackintosh – macOS Tahoe (Working)

⚠️ **This is the `tahoe` branch.** For the macOS Sequoia EFI, switch to the [`sequoia`](https://github.com/icliberen/hackintosh-opencore-sequoia-tahoe-msi_b850m_g-ryzen5_9600x-rx570-rx5500xt) branch.

Working OpenCore EFI for running **macOS Tahoe** on the MSI B850M-G motherboard with an AMD Ryzen 5 9600X and **RX 5500 XT** GPU.

## Hardware Specifications

| Component | Model | Status |
| :--- | :--- | :--- |
| **CPU** | AMD Ryzen 5 9600X (6-Core) | ✅ Working (AMD Vanilla Patches) |
| **Motherboard** | MSI B850M-G | ✅ Working |
| **GPU** | AMD Radeon RX 5500 XT (Navi 14) | ✅ Working (`agdpmod=pikera` required) |
| **Ethernet** | Realtek RTL8126 (Dragon 5GbE) | ✅ Working (`LucyRTL8125Ethernet.kext`) |
| **Storage (macOS)** | WD HDD 500GB partition (USB) | ✅ Working |
| **Storage (Windows)** | Toshiba NVMe SSD (Internal) | ⛔ Disabled via SSDT |
| **Audio** | Realtek ALC897 | ✅ Working (`AppleALC.kext`, `alcid=11`) |

## What Changed from Sequoia (main branch)

| Change | Sequoia (main) | Tahoe (this branch) |
| :--- | :--- | :--- |
| **GPU** | RX 570 (Polaris, native) | RX 5500 XT (Navi 14, `agdpmod=pikera`) |
| **macOS Version** | Sequoia | Tahoe |
| **Ethernet Kext** | Not included (caused KP) | `LucyRTL8125Ethernet.kext` ✅ |
| **Extra Kexts** | — | `CryptexFixup.kext`, `ForgedInvariant.kext`, `RestrictEvents.kext` |
| **USB Mapping** | `USBMap.kext` | `USBToolBox.kext` + `UTBDefault.kext` + `UTBMap.kext` |
| **SSDT-CPUR** | Not included | ✅ Included (B850 CPU mapping) |

## Kexts Included

| Kext | Purpose |
| :--- | :--- |
| `Lilu.kext` | Core patching framework |
| `VirtualSMC.kext` | SMC emulation |
| `WhateverGreen.kext` | GPU patching (required for Navi) |
| `AppleALC.kext` | Audio codec patching |
| `AMFIPass.kext` | AMFI bypass for Tahoe |
| `AppleMCEReporterDisabler.kext` | Prevents MCE panic on AMD |
| `CryptexFixup.kext` | Cryptex OS volume fix |
| `ForgedInvariant.kext` | TSC/FSB frequency fix for AMD |
| `RestrictEvents.kext` | Fixes CPU name and misc events |
| `NVMeFix.kext` | NVMe power management |
| `LucyRTL8125Ethernet.kext` | Realtek 2.5/5GbE driver |
| `USBToolBox.kext` | USB mapping tool |
| `UTBDefault.kext` / `UTBMap.kext` | USB port map |

## BIOS Settings

- **Disable:** Fast Boot, Secure Boot, CSM/Legacy Boot, IOMMU, Re-Size BAR Support
- **Enable:** Above 4G Decoding, XHCI Hand-off, OS Type: UEFI

## Important Notes

1. **RX 5500 XT (Navi 14):** Requires `agdpmod=pikera` in boot-args. WhateverGreen handles all necessary framebuffer patches.
2. **6-Core CPU Patch:** AMD kernel patches adjusted with `ugYAAAA=` to match the Ryzen 5 9600X's 6 cores.
3. **SSDT-CPUR:** Included for proper CPU initialization on B850 chipset.
4. **Ethernet:** `LucyRTL8125Ethernet.kext` now works correctly with the RTL8126 controller under Tahoe.
5. **Boot from Internal SSD:** This EFI is designed to boot directly from the SSD's EFI partition, could boot from both other SSD EFI and Sata SSD EFI (Used Sata SSD because NVME SSD's that are appropriate for hackintosh were hard to find and run) (no USB required).

## Credits

- [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [AMD OS X Community](https://forum.amd-osx.com/)
- [Acidanthera](https://github.com/acidanthera) (Lilu, WhateverGreen, VirtualSMC, OpenCore)
