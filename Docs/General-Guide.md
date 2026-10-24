<div align="center">

# Hackintosh ThinkPad T480s (Aercintosh)

**OpenCore Sequoia & Tahoe EFIs for Lenovo ThinkPad T480s (i7-8650U, Intel UHD 620)**  
Hackintosh build by [aerhazu](https://github.com/aerhazu). Credits to [felikafelix](https://github.com/felikafelix/Hackintosh-Thinkpad-T480s)s' build.

</div>

---

<p align="center">
<img src="Assets/Sequoia/sequoia.png">
</p>

<p align="center">
<a href="https://github.com/aerhazu/Hackintosh-ThinkPad-T480s">
<img src="https://img.shields.io/badge/Sequioa%2015.8.1-%20?style=for-the-badge&logo=apple&label=MacOS&color=327566">
</a>
</p>

<p align="center">

<a href="https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.7">
<img src="https://img.shields.io/badge/1.0.8-%20?style=for-the-badge&logo=singlestore&logoColor=white&label=OpenCore&color=08b2c9">
</a>
<a img src="https://img.shields.io/badge/Status-Stable-brightgreen?style=for-the-badge&logo=github">
</a>

---

> [!WARNING]
> While this EFI significantly reduces setup time, the Hackintosh journey still requires:
> - Understanding basic concepts from the [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/)
> - Testing and troubleshooting during the installation process
> - Patience and persistence in resolving any issues that arise
>
> My EFI does not guarantee a successful installation in the first attempt, but it should help you get started.

---

## 🚀 Quick Start

Before we begin, it is recommended that Windows is already installed and that you have a second NVMe drive available for macOS. If you plan to dual-boot Windows and macOS, staying on Windows 10 is highly recommended, as Windows 11's Secure Boot requirements may cause compatibility issues with certain applications and games.

While in Windows, make sure all firmware and drivers are fully updated, including your Wi-Fi, Bluetooth, chipset, BIOS, and CPU-related firmware.

**For ThinkPad T480s users, updating the Thunderbolt firmware is <u>absolutely critical</u>**. Failure to update the Thunderbolt firmware can result in permanent Thunderbolt controller issues, causing Thunderbolt and USB-C functionality to fail. Verify that the latest Thunderbolt firmware from Lenovo is installed before continuing with the installation process.

### 1. Download EFI
Get the latest EFI from [Releases](https://github.com/aerhazu/Hackintosh-ThinkPad-T480s/r)

### 2. Generate SMBIOS
Use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS):
- **Sequoia**: `MacBookPro16,3`
- **Tahoe**: `MacBookPro16,4` (For best battery management)

### 3. MacOS Download and Installation

<details>
  
<summary>Fresh install (Bootable Drive/.raw)</summary>

#### 1. Prepare Installation Media

1. **Prepare Your USB or NVMe Drive**  
   A 16GB drive is recommended. Download Sequoia/Tahoe macOS image (.raw). You can check [Olarila](https://olarila.com/topic/6278-olarila-vanilla-images-macos-installer/) for your desired version.

2. **Flash with Rufus**  
   - Open Rufus.  
   - Select your USB drive and the downloaded `.raw` file.  
   - Make sure the partition scheme is `GPT`.
   - Click **Start**.  
   ⚠️ Warning: This will erase the USB completely.

#### 2. Configure the EFI Partition (Diskpart Method)

Since the EFI partition is hidden in Windows, use diskpart to mount it and replace the bootloader files.

1. Open CMD as Administrator and run: `diskpart`

2. Mount the partition (Enter these one by one)  

| Command | Description |
|-----|-------|
| `list disk`  | Identify your USB disk number (e.g., Disk 2)   |
| `select disk X`  | Replace **X** with your USB disk number     |
| `list partition` | Locate the EFI/System partition (~200MB) |
| `select partition Y`  | Replace **Y** with the EFI partition number |
| `assign letter=Z`  | Mount the partition as drive `Z:` or pick another letter |  
---

3. Replace EFI Folder:
   - Download [Explorer++](https://explorerplusplus.com/)
   - Open Explorer++ (Run as Admin) to access the S: drive.
   - Delete the existing EFI folder on the USB. (If there are any)
   - Copy the new EFI folder
   - Paste your new EFI folder onto the root of the Z: drive.
   - Eject your drive.

#### 3. Configure BIOS
See [BIOS Settings](#-bios-settings) section below.

#### 4. macOS Installation
- Shutdown your PC.
- Boot: Press your PC's boot menu key (F12 for ThinkPad) and select your MacOS installer USB drive.
- OpenCore Bootloader: Select Install MacOS
- Disk Utility:
  - Select Disk Utility from the macOS Recovery menu.
  - Select your target SSD.
  - Click Erase and format as:
      - Name: Macintosh HD
      - Format: APFS (or Mac OS Extended Journaled for older versions)
      - Scheme: GUID Partition Map
      - Install: Close Disk Utility, select Install macOS, and follow the prompts. The system will reboot several times.
  
</details>

### 4. Post-Install
See **[Post-Install Guide](https://github.com/aerhazu/Hackintosh-ThinkPad-T480s/blob/main/Docs/Post-Install.md)** for:
- Boot Chime
- Undervolting setup
- Display scaling (HiDPI / BetterDisplay)
- YogaSMC for Fn keys
- WiFi patching (AirportItlwm or itlwm)
- Airdrop/Continuity fix for itlwm users
- Etc...

---

## 🪛 BIOS Settings

| Menu Path |                                                     Setting |
|:----------|------------------------------------------------------------:|
| Config > USB > Always On USB |   **Enabled**<br/>*(Required for Thunderbolt<br/>port to work!)* |
| Config > Keyboard/Mouse > Trackpoint |                                        **Enabled/Disabled** |
| Config > Keyboard/Mouse > Trackpad |                                                 **Enabled** |
| Config > Keyboard/Mouse > Fn and Ctrl Key swap |                                                **Disabled** |
| Config > Keyboard/Mouse > Fn Sticky Key |                                                **Disabled** |
| Config > Keyboard/Mouse > F1-F12 as Primary Function |                                                **Disabled** |
| Config > Display > Boot Display Device |                                            **Thinkpad LCD** |
| Config > Display > Shared Display Priority |                                              **USB Type-C** |
| Config > Display > Total Graphics Memory |                                                   **512MB** |
| Config > Power > Intel SpeedStep Technology |                                                 **Enabled** |
| Config > CPU > Intel Hyper-Threading Technology |                                                 **Enabled** |
| Config > Thunderbolt 3 > Thunderbolt BIOS Assist Mode |                                                **Disabled** |
| Config > Thunderbolt 3 > Security Level |                                             **No Security** |
| Config > Thunderbolt 3 > Wake by Thunderbolt 3 |                                                 **Enabled** |
| Config > Thunderbolt 3 > Support in Pre Boot Environment |                                                 **Enabled** |
| Security > Security Chip |  **Disabled**<br/>*(Enable if dualbooting<br/>with Windows* |
| Security > Memory Protection > Execution Prevention |                                                 **Enabled** |
| Security > Virtualization > Intel VT |                                                 **Enabled** |
| Security > Virtualization > Intel VT-d |                                                 **Enabled** |
| Security > I/O Port Access > Fingerprint Reader | **Disabled**<br/>*(Enable if dualbooting<br/>with Windows)* |
| Security > Secure Boot |                                                **Disabled** |
| Security > Intel SGX |                                     **Software Controlled** |
| Security > Device Guard |                                                **Disabled** |
| Boot > UEFI/Legacy Boot |                                                    **Both** |
| Boot > UEFI/Legacy Boot Priority |                                            **Legacy First** |
| Boot > CSM Support |                                                     **Yes** |

---

## 📸 Screenshots

<details>
<summary><b>Click to expand gallery</b></summary>

### Desktop
<p align="center"><img src="Assets/Sequoia/desktop.png"></p>

### Wifi
<p align="center"><img src="Assets/Sequoia/wifi.png"></p>

### Bluetooth
<p align="center"><img src="Assets/Sequoia/bluetooth.png"></p>

</details>
