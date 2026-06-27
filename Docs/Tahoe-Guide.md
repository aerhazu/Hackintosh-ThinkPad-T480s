<div align="center">

# Hackintosh ThinkPad T480s (Aercintosh)
**OpenCore EFI for Lenovo ThinkPad T480s (i7-8650U, Intel UHD 620)**  
Hackintosh build by [aerhazu](https://github.com/aerhazu). Credits to [felikafelix](https://github.com/felikafelix/Hackintosh-Thinkpad-T480s)s' build.

</div>

---

<p align="center">
<a href="https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.7">
<img src="https://img.shields.io/badge/1.0.7-%20?style=for-the-badge&logo=singlestore&logoColor=white&label=OpenCore&color=08b2c9">
</a>
<a img src="https://img.shields.io/badge/Status-Stable-brightgreen?style=for-the-badge&logo=github">
</a>
</p>

<p align="center">

<a href="https://github.com/aerhazu/Hackintosh-ThinkPad-T480s">
<img src="https://img.shields.io/badge/Sequioa%2015.7.7-%20?style=for-the-badge&logo=apple&label=MacOS&color=327566">
</a>
<a href="https://github.com/aerhazu/Hackintosh-ThinkPad-T480s">
<img src="https://img.shields.io/badge/Tahoe%2026.5.1-%20?style=for-the-badge&logo=apple&label=MacOS&color=53a0f3">
</a>

<p align="center">
<img src="Documentation/Assets/Sequoia/about.png">
</p>

---

> [!NOTE]
> This EFI is a quick-start baseline for the Lenovo ThinkPad T480s (i7-8650U, Intel UHD 620), built from real Hackintosh experience to reduce setup time.
>
> 🚨 **Please Note:**
> - Always review and generate your own serial numbers (SystemSerialNumber, MLB, UUID) before signing into Apple ID to avoid potential service locks.

> [!WARNING]
> While this EFI significantly reduces setup time, the Hackintosh journey still requires:
> - Understanding basic concepts from the [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/)
> - Testing and troubleshooting during the installation process
> - Patience and persistence in resolving any issues that arise
>
> My EFI does not guarantee a successful installation in the first attempt, but it should help you get started.

---

## 🚀 Quick Start

### 1. Download EFI
Get the latest EFI from [Releases](https://github.com/aerhazu/Hackintosh-ThinkPad-T480s/releases)

### 2. Generate SMBIOS
Use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS):
- **Sequoia**: `MacBookPro16,3` (For best battery management)

### 3. MacOS Download and Installation

<details>
  
<summary>Sequoia (Bootable Drive/.raw)</summary>

#### 1. Prepare Installation Media

1. **Prepare Your USB or NVMe Drive**  
   A 16GB drive is recommended. Download Sequoia macOS image (.raw). You can check [Olarila](https://olarila.com/topic/6278-olarila-vanilla-images-macos-installer/) for your desired version.

2. **Flash with Rufus**  
   - Open Rufus.  
   - Select your USB drive and the downloaded `.raw` file.  
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
See (BIOS Settings)[#-bios-settings] section below.

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

<details>
  
<summary>Tahoe (COMING SOON)</summary>
If there is enough demand for a Tahoe build, I can prepare and release one. However, for everyday use, Sequoia remains the more stable and practical choice. It offers better overall compatibility, fewer unexpected issues, and more mature support within the Hackintosh ecosystem.

</details>

### 4. Post-Install
See **[Post-Install Guide]()** for:
- Boot Chime
- Undervolting setup
- Display scaling (HiDPI / BetterDisplay)
- YogaSMC for Fn keys
- WiFi patching (AirportItlwm or itlwm)
- Airdrop/Continuity fix for itlwm users
- Etc...

---

## 💻 Device Specification

<details><summary>Click to see more</summary>

| Component | Details |
|----------:|:--------|
| **Model** | Lenovo ThinkPad T480s |
| **CPU** | Intel Core i7-8650U (2.11GHz, Turbo 4.2GHz) |
| **iGPU** | Intel UHD Graphics 620 |
| **RAM** | 2×8GB DDR4 2400MHz |
| **SSD** | GUDGA NE-512 M.2 2242 512GB |
| **Display** | IPS 14" FHD 1920×1080 & Xiaomi A24i 2026 (External) |
| **Audio** | Realtek ALC257 |
| **Battery** | No internal battery |
| **Ethernet** | Intel I219-LM4 |
| **WiFi/BT** | Intel Wireless-AC 8265/8275 |
| **Camera** | 720p HD |
| **Trackpad** | Synaptics Precision (PS2/SMBus) |

</details>

---

## 🪛 BIOS Settings

<details><summary>Click to see more</summary>

| Menu Path | Setting |
|:----------|--------:|
| Config > USB > Always On USB | **Disabled** |
| Config > Keyboard/Mouse > Trackpoint | **Enabled** |
| Config > Keyboard/Mouse > Trackpad | **Enabled** |
| Config > Keyboard/Mouse > Fn and Ctrl Key swap | **Disabled** |
| Config > Keyboard/Mouse > Fn Sticky Key | **Disabled** |
| Config > Keyboard/Mouse > F1-F12 as Primary Function | **Disabled** |
| Config > Display > Boot Display Device | **Thinkpad LCD** |
| Config > Display > Shared Display Priority | **USB Type-C** |
| Config > Display > Total Graphics Memory | **512MB** |
| Config > Power > Intel SpeedStep Technology | **Enabled** |
| Config > CPU > Intel Hyper-Threading Technology | **Enabled** |
| Config > Thunderbolt 3 > Thunderbolt BIOS Assist Mode | **Enabled** |
| Config > Thunderbolt 3 > Security Level | **No Security** |
| Config > Thunderbolt 3 > Wake by Thunderbolt 3 | **Enabled** |
| Config > Thunderbolt 3 > Support in Pre Boot Environment | **Enabled** |
| Security > Security Chip | **Disabled** |
| Security > Memory Protection > Execution Prevention | **Enabled** |
| Security > Virtualization > Intel VT | **Enabled** |
| Security > Virtualization > Intel VT-d | **Enabled** |
| Security > I/O Port Access > Fingerprint Reader | **Disabled** |
| Security > Secure Boot | **Disabled** |
| Security > Intel SGX | **Software Controlled** |
| Security > Device Guard | **Disabled** |
| Boot > UEFI/Legacy Boot | **Both** |
| Boot > UEFI/Legacy Boot Priority | **Legacy First** |
| Boot > CSM Support | **Yes** |

</details>

---

## 📊 Status

<details>
<summary><b>✅ Working</b></summary>

| Feature | Notes |
|---------|-------|
| QE/CI & Hardware Acceleration | IQSV fully supported |
| Battery Management | Accurate percentage |
| CPU Power Management | Performance optimized |
| USB-A & USB-C | Including power delivery |
| HDMI Output | Video & audio |
| Audio | Speaker, internal mic, 3.5mm jack |
| WiFi 5GHz & 2.4GHz | Supported with Airdrop, Continuity, etc. |
| Bluetooth | Fully functional |
| Ethernet | Intel I219-LM |
| Trackpad & Trackpoint | Full gesture support |
| Touchscreen | Same as Trackpad, multi-gesture support |
| Keyboard & Backlight | All keys working |
| Internal Webcam | 720p HD |
| Sleep/Wake | Stable |
| ThinkPad Fn Keys | F1-F12 via YogaSMC |
| iServices | iMessage, FaceTime, App Store, Find My |
| Apple Music | Lossless/Hi-Res supported |
| Airdrop | Send to iPhone only |
| Continuity/Handoff | Universal Clipboard, etc. |
| iPhone Camera | USB cable |
| Android USB Tethering | HoRNDIS included |

</details>

<details>
<summary><b>❌ Not Working</b></summary>

| Feature | Reason |
|---------|--------|
| Safari DRM / Apple TV+ | Requires dGPU (workaround: Chrome/Firefox with Widevine) |
| Fingerprint Reader | No macOS driver |
| Airdrop Receiver | Intel WiFi limitation |
| iPhone Camera (Wireless) | Requires native AirDrop |

</details>

<details>
<summary><b>🔍 Not Tested</b></summary>

- Thunderbolt 3 (no device to test)
- Card Reader (no memory card)
- WWAN (no card installed)

</details>

<details>
<summary><b>🐛 Known Issues</b></summary>

**Color Banding**
-  Inaccurate color gradients in subtle shades (skies, dark scenes, UI gradients). This is a T480s stock panel limitation. Workaround by spoofing to Skylake platform-id works only on Monterey and older.

**ELAN Microelectronics USB Touchscreen Variant Issue**
- For ELAN Microelectronics USB Touchscreen variant on Tahoe, the touchscreen works but causes issues with wake from sleep, restart, and shutdown.


</details>

---

## 📸 Screenshots

<details>
<summary><b>Click to expand gallery</b></summary>

### Desktop
<p align="center"><img src="Documentation/Assets/Desktop.png"></p>

</details>
