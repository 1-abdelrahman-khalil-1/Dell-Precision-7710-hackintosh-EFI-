# Dell Precision 7710 Hackintosh EFI (macOS Monterey)

This EFI is configured for **Dell Precision 7710** running **macOS Monterey** using **OpenCore**.  
Intel HD 530 is fully accelerated with 1536MB VRAM.  
NVIDIA Quadro M3000M is disabled completely to avoid black screen or kernel panic.

---

## ✅ Hardware

| Component       | Model                      |
|----------------|----------------------------|
| CPU            | Intel Core i7-6820HQ       |
| iGPU           | Intel HD Graphics 530      |
| dGPU           | NVIDIA Quadro M3000M (Disabled) |
| RAM            | 16 GB DDR4                 |
| SSD            | M.2 NVMe 512GB             |
| macOS Version  | Monterey                   |

---

## 🛠️ Required BIOS Settings

- **Disable** Secure Boot
- **Disable** Legacy ROMs
- **Set SATA Mode** to AHCI
- **Enable** UEFI Boot
- **Disable** Fast boot
- **Disable** VT-d (or use `DisableIoMapper` Quirk)
- **No DVMT pre-allocation option available in BIOS**

---

## 🔧 GRUB Mod to Disable dGPU + Set DVMT to 96MB

### 📁 Included DVMT folder

This repo includes a folder: `DVMT/`  
Inside it:  
`boot/DVMT/bootx64.efi`

Use this to disable NVIDIA dGPU and set proper iGPU memory for macOS.

### 🪛 Steps

1. Format a USB drive as **FAT32**
2. Copy the **entire** `DVMT` folder to the root of the USB, so the path becomes:  
   `USB:/DVMT`
3. Reboot your laptop and enter BIOS
4. Add a new boot entry and point it to:  
   `\boot\DVMT\bootx64.efi`
5. Boot from this new boot option

---

### ⚙️ What You’ll See in GRUB Menu

- Select the **first option**:  
  > `#1 IGPU-only configuration. No dGPU.`

This will:
- Disable the NVIDIA Quadro M3000M completely (no longer visible to macOS or Windows)
- Set **DVMT Pre-Allocated** to `96MB` for proper iGPU memory allocation

You only need to do this once.

---

### 🔁 Restore dGPU for Windows (Optional)

If you ever need to use the NVIDIA GPU again (for Windows, for example):

1. Boot into the **GRUB** USB again
2. Select the **last option**:  
   > `# Restore Dell 7510/7710 UEFI variables back to factory settings`

This will re-enable the NVIDIA GPU and reset all changes.

---

## 🧠 OpenCore Boot Args

Recommended boot arguments (already applied):
-v -disablegfxfirmware debug=0x100 keepsyms=1

These:
- Disable loading Intel GFX firmware (which causes black screen or retry loops)
- Fix VRAM issue (7MB bug)
- Allow kernel debugging (for advanced users)

---

## 📈 GPU Patching (DeviceProperties)

- Platform ID: `0x1B190000` (AAPL,ig-platform-id)
- `framebuffer-patch-enable` = `01000000`
- `framebuffer-fbmem` = `00009000`
- `framebuffer-stolenmem` = `00003001`
- `AAPL,ig-platform-id` = `00001B19`

Result: Intel HD 530 with full acceleration and 1536MB VRAM

---

## ✅ What’s Working

- [x] Intel HD 530 QE/CI (1536MB)
- [x] Brightness control
- [x] Audio
- [x] Trackpad + Keyboard
- [x] USB 3.0 (custom mapped)
- [x] Sleep/Wake (partial, needs testing)
- [x] Wi-Fi (if compatible card used)

---

## ⚠️ Notes

- If you're seeing **"Hash data from ME never returned"** in boot logs — it's harmless with `-disablegfxfirmware`.
- Make sure to reset NVRAM from OpenCore boot menu **after installing this EFI**.

---

## 💬 Credits

- Dortania OpenCore Guide  
- Hackintosh Community (Reddit, InsanelyMac)  
- @thatsitdave for GRUB mod  
- Everyone who contributed to Dell 7710 Hackintosh success

---

