# Post-Install Guide

This guide covers all essential post-installation steps for your Hackintosh ThinkPad T480s. Most steps are from [felikafelix](https://github.com/felikafelix/Hackintosh-Thinkpad-T480s)s' repo, some steps are modified/simplified in a way that's much easy to do.

---

## Table of Contents
- [Undervolting](#-undervolting)
- [Display Scaling](#-display-scaling)
- [YogaSMC Setup](#-yogasmc-setup)
- [Audio Setup (Tahoe)](#-audio-setup-macos-tahoe-only)
- [USB Mapping](#-usb-mapping)
- [CPUFriendDataProvider](#-cpufrienddataprovider)
- [Wireless Setup](#-wireless-setup)
   - Option A: AirportItlwm + OCLP Patch
   - Option B: itlwm + Heliport (Tested on Tahoe 26.2)
- [Airdrop & Continuity Fix](#-airdrop-and-continuity-fix-itlwm-users)

---

## ⚡ Undervolting

For better thermal performance and battery life, use [VoltageShift](https://github.com/sicreative/VoltageShift). The required kext is already included in the EFI.

### Recommended Values
| Component | Offset |
|-----------|--------|
| CPU | -135 |
| GPU | -140 |
| CPU Cache | -40 |
| PL1 | 22W |
| PL2 | 44W |

### 1. Install VoltageShift
1. Download VoltageShift from the [GitHub repository](https://github.com/sicreative/VoltageShift) and extract the archive.
2. Open Terminal and run the following command:
   ```
   sudo cp ~/Downloads/VoltageShift/voltageshift /usr/local/bin/
   ```
**Note:** This command assumes the extracted `VoltageShift` folder is located in your Downloads folder. If you extracted it elsewhere or the folder has a different name, change the path in the command accordingly.
4. Make the executable runnable:
   ```
   sudo chmod +x /usr/local/bin/voltageshift
   ```
4. Verify the installation:
   ```
   which voltageshift
   ```
   The expected output is `/usr/local/bin/voltageshift`.

### 2. Apply an Undervolt

**With Turbo Boost Enabled**  
Maximum performance at the expense of battery life and potentially higher temperatures. Recommended for systems without an internal battery or those primarily used while plugged in.

```bash
sudo ./voltageshift buildlaunchd -135 -140 -40 0 0 0 1 0 0 1 160 && voltageshift power 22 44
```

**With Turbo Boost Disabled**  
Balanced performance, power consumption, and battery life. Recommended for systems with an internal battery.

```bash
sudo ./voltageshift buildlaunchd -135 -140 -40 0 0 0 0 0 0 1 160 && voltageshift power 22 44
```

### Testing
After setting offsets, stress test your system:
- Open multiple browser tabs
- Play 4K video
- Run benchmark tools

If the laptop crashes/freezes, force restart. Values revert to default (0 0 0).

---

## 🖥️ Display Scaling

Choose one of the following methods to enable HiDPI display scaling.

### Option 1: HiDPI Script

1. Download and run the [one-key-hidpi script](https://github.com/xzhih/one-key-hidpi).
2. Select the following options:
   - `(2) Enable HiDPI (With EDID)`
   - `(3) MacBook Pro`
   - `(1) 1920x1080 Display`
3. Reboot your system.

### Option 2: BetterDisplay (Recommended)

1. Install [BetterDisplay](https://github.com/waydabber/BetterDisplay).
2. Enable HiDPI mode.
3. Set a custom resolution according to your display and preferences.

---

## 🎛️ YogaSMC Setup

Install YogaSMC for additional ThinkPad-specific function key support and hardware controls.

1. Install the [YogaSMC application](https://github.com/zhen-zen/YogaSMC).
2. Use the YogaSMCNC menu bar app to manage supported features, including:
   - Fan control
   - LED lights
   - Function keys

### Fn Key Functions

| Key | Function |
| --- | --- |
| F1 | Mute Speaker |
| F2 / F3 | Volume Down / Up |
| F4 | Mute Microphone |
| F5 / F6 | Brightness Down / Up |
| F7 | Switch Display |
| F8 | Toggle Wi-Fi* |
| F9 | Preferences |
| F10 | Toggle Bluetooth |
| F11 | Toggle Keyboard |
| F12 | Launchpad |

\* F8 Wi-Fi toggling does not work with `itlwm` and HeliPort.

---

## 🔊 Audio Setup (macOS Tahoe Only)

Before proceeding, ensure that `AppleALC.kext` is installed in your EFI configuration and that the correct `layout-id` is configured.

### Option A: MyKextInstaller

1. Ensure `AMFIPass.kext` is enabled, or add `amfi=0x80` to your boot arguments.
2. Download [MyKextInstaller](https://github.com/Mirone/MyKextInstaller/releases).
3. Open MyKextInstaller, select **Download KDKs**, and install the required Kernel Debug Kit.
4. Select **Install Kexts** and install `AppleHDA.kext`.
5. Reboot your system.

### Option B: OCLP-MOD

1. Download [OCLP-MOD](https://github.com/laobamac/OCLP-Mod/releases).
2. Open OCLP-MOD and check for available audio patches, alongside any applicable modern wireless or Intel wireless patches.
3. Install the required patches.
4. Reboot your system.

---

## 🔌 USB Mapping

Create a custom USB map to improve port compatibility and ensure USB ports function correctly across sleep, wake, and normal operation.

Choose one of the following tools:

1. [USBToolBox](https://github.com/USBToolBox/tool)
2. [USBMap](https://github.com/corpnewt/USBMap)
3. [Hackintool](https://github.com/benbaker76/Hackintool) — use the USB section to configure and map your ports.

---

## ⚙️ CPUFriendDataProvider

Generate a custom power profile:

1. Download [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend).
2. Run it and choose a preference:
   - Prioritize Power
   - Balanced Power
   - Balanced Performance
   - Prioritize Performance
3. Add the generated kext to your EFI.


**OR**

Choose from the provided presets [here](https://github.com/aerhazu/Hackintosh-ThinkPad-T480s/tree/main/Presets/CPUFriend%20Presets). (All required kexts are already in the EFI)

---

## 📶 Wireless Setup

> [!NOTE]  
> **These steps are already done in the EFI**. You may only need to install the patch using OCLP Patcher or OCLP Modded Patcher. This requires further testing. Please open an issue if it doesn't work.

On Sequoia and Tahoe, choose one of the following options.

### Option A: AirportItlwm + OCLP Patch

> Works like native Wi-Fi and supports iServices and one-way AirDrop.

#### Prerequisites

- [Hackintool](https://github.com/benbaker76/Hackintool/releases)
- OpenCore Legacy Patcher: [Official](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) for Sequoia or [Modded](https://github.com/laobamac/OCLP-Mod) for Tahoe
- [ProperTree](https://github.com/corpnewt/ProperTree) or another `config.plist` editor
- [IO80211FamilyLegacy.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Wifi/IO80211FamilyLegacy-v1.0.0.zip)
- [IOSkywalkFamily.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Wifi/IOSkywalkFamily-v1.2.0.zip)
- [AMFIPass.kext](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AMFIPass-v1.4.1-RELEASE.zip)
- [AirportItlwm.kext](https://github.com/openintelwireless/itlwm/releases) — **Get the Ventura version.**

#### Step 1: Configure `config.plist`

> Spoofing is required for Sequoia when using the official OCLP version.

> If you use the modded OCLP version on Sequoia or Tahoe, skip spoofing. The modded version provides a patch for Intel wireless cards.

1. Open Hackintool and go to **PCIe**.
2. Find your Intel wireless card, right-click it, and select **Copy Device Path**.

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport1.png" width="600"></p>

3. Open `config.plist` in ProperTree.
4. Go to `DeviceProperties > Add`.
5. Add a new dictionary using your device path. This spoofs the wireless card. **Official OCLP users must do this; modded OCLP users should skip this step.**

| Key | Type | Value |
|-----|------|-------|
| IOName | String | pci14e4,43a0 |
| compatible | String | pci106b,117 |
| device-id | Data | A0430000 |
| device_type | String | Network Controller |
| model | String | BCM4360 802.11ac Wireless Network Adapter |
| name | String | pci14e4,43a0 |
| pci-aspm-default | Number | 0 |
| subsystem-id | Data | 17010000 |
| subsystem-vendor-id | Data | 6B100000 |
| vendor-id | Data | E4140000 |

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport2.png" width="600"></p>

6. Add the kexts to `EFI/OC/Kexts`, then use ProperTree’s `⌘+R` shortcut to take a snapshot.

**Kext order:**

| # | Kext |
|---|------|
| 1 | IOSkywalkFamily.kext |
| 2 | IO80211FamilyLegacy.kext |
| 3 | IO80211FamilyLegacy.kext/Contents/Plugins/AirPortBrcmNIC.kext |
| 4 | AMFIPass.kext |
| 5 | AirportItlwm.kext |

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport3.png" width="600"></p>

7. Go to `Kernel > Block` and enable **Allow IOSkywalk Downgrade**.

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport4.png" width="600"></p>

8. Set `NVRAM > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > csr-active-config` to `03080000`.

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport5.png" width="600"></p>

9. Modded OCLP users should add `-amfipassbeta` to the boot arguments.
10. **Reboot.**

#### Step 2: Apply the OCLP patch

1. Open OpenCore Legacy Patcher.
2. Select **Post-Install Root Patch**. Available patches should appear for modern wireless (official OCLP) or Intel wireless (modded OCLP).
3. Click **Start Root Patching**.
4. When patching is complete, reboot.

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/oclp1.png" width="400"></p>

> If you encounter SIP errors, try resetting NVRAM.

#### Step 3: Disable spoofing (official OCLP only)

Modded OCLP users should skip this step.

1. Open `config.plist`.
2. In `DeviceProperties > Add`, prefix your device path with `#`.
   - Example: `#PciRoot(0x0)/Pci(0x1C,0x6)/Pci(0x0,0x0)`

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport6.png" width="600"></p>

3. Reboot. Wi-Fi should now work.

#### Step 4: Fix Bluetooth

Add these entries under `NVRAM > 7C436110-AB2A-4BBB-A880-FE41995C9F82`:

| Key | Type | Value |
|-----|------|-------|
| bluetoothExternalDongleFailed | Data | 00 |
| bluetoothInternalControllerInfo | Data | 0000000000000000000000000000 |

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/airport7.png" width="600"></p>

If Bluetooth still does not work, try resetting NVRAM more than once.

---

### Option B: itlwm + Heliport

> This setup is simpler, but requires the HeliPort app to connect to Wi-Fi.

> Tested on Tahoe 26.2.

> After upgrading to 26.3, AirDrop stopped working with this method. Consider using the OCLP method instead.

1. Disable all AirportItlwm-related kexts.
2. Disable **Allow IOSkywalk Downgrade** under `Kernel > Block`.
3. Set `csr-active-config` to `00000000`.
4. Enable `itlwm.kext` in `config.plist`.
5. Install [HeliPort](https://openintelwireless.github.io/HeliPort/Installation.html).
6. Add HeliPort to **Login Items**.

---

## 🪂 AirDrop and Continuity Fix (itlwm Users)

This patch enables AirDrop, Continuity Handoff, Private Relay, and other features for itlwm + HeliPort users.

> This can work without an iCloud+ subscription. Private Relay will show “Unavailable,” but the other features may still work.

> Tested on Tahoe 26.2.

> After upgrading to 26.3, AirDrop stopped working with this method. Consider using the OCLP method instead.

### Prerequisites

- itlwm.kext and HeliPort configured
- Private Relay turned on, even if its status is `Unavailable`
- [Cloudflare WARP for macOS](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/warp/download-warp/)
- No other third-party VPN or tunnel app connected or running, since Private Relay may fail with a `software in this mac is incompatible with private relay` error
   - You can still use a third-party VPN without Private Relay. To reactivate Private Relay, disconnect the VPN app and repeat the workaround toggle.
- Private Relay requires an iCloud+ subscription. Without one, the workaround may still enable AirDrop, Continuity, and related features without Private Relay. (Tested after the subscription expired.)

### Features supported by this workaround

- AirDrop send only (expected; AirportItlwm behaved the same way on Sequoia and Ventura)
- Continuity Handoff
- Universal Clipboard, one-way (expected; AirportItlwm behaved the same way on Sequoia and Ventura)
- Location Services at low accuracy

### Setup

1. Install Cloudflare WARP for macOS.
2. In **System Settings > General > Login Items & Extensions**:
   - Enable **Cloudflare Inc.**
   - Disable **Cloudflare WARP.app** (optional)

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/cloudflareitem.png" width="400"></p>

3. Create the automation script:

```bash
sudo su
cd /usr/local/bin
touch enable_privaterelay.sh
chmod +x enable_privaterelay.sh
```

4. Edit `/usr/local/bin/enable_privaterelay.sh`:

```bash
#!/bin/bash

# --- CONFIGURATION ---
TARGET_USER="YOUR_USERNAME_HERE"
CF_PLIST="/Library/LaunchDaemons/com.cloudflare.1dot1dot1dot1.macos.warp.daemon.plist"
CF_SERVICE="system/com.cloudflare.1dot1dot1dot1.macos.warp.daemon"

# Log for monitoring
log_msg() { echo "$(date): $1" >> /tmp/cf_automation.log; }

log_msg "--- Script started ---"

# 0. Wait for Desktop (Wait for Finder)
while ! pgrep -u "$TARGET_USER" "Finder" > /dev/null; do
    sleep 2
done

log_msg "Desktop active. Waiting 10 seconds..."
sleep 10

# --- STEP 1: ENSURE CLOUDFLARE ENABLED ---
if launchctl list | grep -q "com.cloudflare.1dot1dot1dot1.macos.warp.daemon"; then
    log_msg "[OK] Cloudflare Inc already Enabled."
else
    log_msg "[FIX] Cloudflare Inc Disabled. Enabling..."
    launchctl bootstrap system "$CF_PLIST"
    sleep 2
fi

# --- STEP 2: ENSURE PRIVATE RELAY ON ---
PR_STATUS=$(sudo -u "$TARGET_USER" defaults read com.apple.NetworkServiceProxy NspEnabled 2>/dev/null)

if [ "$PR_STATUS" == "1" ]; then
    log_msg "[OK] Private Relay already ON."
else
    log_msg "[FIX] Private Relay OFF. Enabling..."
    sudo -u "$TARGET_USER" defaults write com.apple.NetworkServiceProxy NspEnabled -bool true
    sleep 2
fi

# --- STEP 3: TOGGLE CLOUDFLARE ---
log_msg "[ACTION] Toggling Cloudflare Inc..."

# Disable
launchctl bootout "$CF_SERVICE" 2>/dev/null
log_msg "-> Disabled"
sleep 3

# Enable
launchctl bootstrap system "$CF_PLIST"
launchctl kickstart -k "$CF_SERVICE" 2>/dev/null
log_msg "-> Enabled"

log_msg "--- Done ---"
```

5. Replace `YOUR_USERNAME_HERE` in `TARGET_USER` with your username. Run `whoami` to check it.

### Usage options

#### Option 1: Manual (Shortcuts app)

1. Open the **Shortcuts** app.
2. Create a new shortcut.
3. Add a **Run Shell Script** action.
4. Enter `/usr/local/bin/enable_privaterelay.sh`.
5. Check **Run as Administrator**.
6. Add the shortcut to the Dock for quick access.

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/guides/shortcut.png" width="500"></p>

#### Option 2: Automatic (LaunchDaemon + Sleepwatcher)

1. Install Sleepwatcher:

```bash
brew install sleepwatcher
```

2. Create a LaunchDaemon:

```bash
sudo nano /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>Label</key>
<string>de.bernhard-baehr.sleepwatcher</string>

<key>ProgramArguments</key>
<array>
    <string>/bin/bash</string>
    <string>-c</string>
    <string>/usr/local/bin/enable_privaterelay.sh; /usr/local/sbin/sleepwatcher -V -w /usr/local/bin/enable_privaterelay.sh</string>
</array>

<key>RunAtLoad</key>
<true/>
<key>KeepAlive</key>
<true/>
</dict>
</plist>
```

3. Start the service:

```bash
sudo launchctl bootstrap system /Library/LaunchDaemons/de.bernhard-baehr.sleepwatcher.plist
```

4. Reboot. The script should run automatically at startup and after waking from sleep.

### Demo

<details>
<summary>Click to see demos</summary>

#### Private Relay

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/demos/privaterelay.gif" width="600"></p>

#### AirDrop (Send to iPhone)

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/demos/airdrop.gif" width="600"></p>

#### Handoff

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/demos/handoff.gif" width="600"></p>

#### Universal Clipboard

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/demos/universalclipboard.gif" width="600"></p>

#### Location Services

<p align="center"><img src="https://github.com/felikafelix/Hackintosh-Thinkpad-T480s/blob/master/assets/demos/location.gif" width="600"></p>

</details>

---

> **Tip:** Use both options: the automation for daily use and the shortcut as a backup if Private Relay disconnects.