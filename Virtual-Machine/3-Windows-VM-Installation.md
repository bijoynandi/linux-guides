# Download Windows 11 ISO 🪟
Visit: https://www.microsoft.com/software-download/windows11

**Steps**: 🚶‍♂️
1. Open Firefox in Ubuntu 🦊
2. Navigate to Microsoft's website
3. Download "Media Creation Tool" or direct ISO
4. **Size**: ~5.4 GB 💾
5. **Note**: You'll need a valid Windows license key 🔐

# 🪟 Creating Your Windows 11 VM - The Ultimate Adventure

## Phase 1: VM Creation Wizard 🧙‍♂️

1. **Start the Journey**: Click "Create a new virtual machine" (the computer icon with +) ➕
2. **Choose Media**: Select "Local install media" → Next 📀
3. **Find Your ISO**: Click "Browse" → Browse Local → navigate to Downloads → select your Windows 11 ISO → Open 📂
4. **OS Recognition**: For "Operating System" type: **Windows 11** 🪟
5. **Confirm**: Click 👍 **Yes** when prompted

## Phase 2: Resource Allocation 💪

### Memory and CPU Power 🧠⚡
- **Memory**:
  - 16GB+ RAM (recommended) 🚀
  
- **CPUs**:
  - 4 cores if you have 8+ cores (smooth sailing, your i3-10100 is plenty) 🌊

### Storage Space 💾
- **Size**: 180 GB (qcow2 format - grows as needed) 📈
- **Type**: qcow2 (efficient and expandable) 🔧

### Final VM Setup 🎯
- **Name**: Windows 11 Pro (or whatever makes you happy) 📝
- **Important**: Check ✅ **"Customize configuration before install"** ⚙️
- Click **Finish** 🏁

---

# ⚙️ Pre-Install VM Configuration - The Secret Sauce

## Overview Settings 📋
**Location**: Click "Overview" on the left
- **Chipset**: Q35 (modern chipset, better than old i440FX) 🆕
- **Firmware**: UEFI x86_64: ...CODE_4M.**secboot**.fd 🔐
  - **Why secboot?** Meets Windows 11 minimum security requirements! 🛡️

## CPU Configuration 🔥
**Location**: Click "CPUs" on the left
- Check ✅ **"Copy host CPU configuration"** 🖥️
- **Result**: VM gets same CPU features as your host (maximum compatibility) ⚡

## Video Settings 🎨
**Location**: Click "Video" on the left
- **Model**: QXL (better for general use than default) 🖼️
- **Video RAM**: 16384 (16MB for smooth graphics) 💾
- **Listen type**: Address 📍
- **Address**: All interfaces 🌐

## Network Configuration 🌐
**Location**: Click "NIC" on the left
- **Network source**: Virtual network 'default': NAT 🏠
- **Device model**: **virtio** (much better performance than default) 🚀
  - **What's virtio?** Paravirtualized drivers for better VM performance! ⚡

## Apply and Launch 🚀
1. Click **"Apply"** ✅
2. Click the **Play button (▶️)** to start your VM adventure! 🎮

---

# 🔧 Installing Windows 11 - The Main Event

## Standard Installation Flow 📋
1. **Boot Process**: VM starts and loads Windows installer 🌅
2. **Language Setup**: Select language, time, keyboard ⌨️
3. **Installation**: Click "Install Now" ▶️
4. **Product Key**: Skip for now (or enter if you have one) 🔑
5. **Version**: Select Windows 11 version 🪟
6. **License**: Accept license terms ✅
7. **Installation Type**: Choose "Custom installation" 🛠️
8. **Disk Selection**: Select the disk → Next 💾
9. **Wait**: Installation takes 15-30 minutes ⏳

## The Network Bypass Trick 🎭

When Windows demands network connection, use this **secret Microsoft backdoor**:

1. Press **Shift + F10** (opens command prompt) 💻
2. Type: `OOBE\BYPASSNRO` and press Enter ⌨️
3. Computer restarts automatically 🔄
4. Go through setup again - now you'll have **"I don't have internet"** option! 🌐❌

## 🎪 The Magic Behind OOBE\BYPASSNRO

**What it stands for**:
- **OOBE** = "Out Of Box Experience" (Windows setup process) 📦
- **BYPASSNRO** = "Bypass Network Requirement Online" 🌐🚫

**The Beautiful Irony**: 😂
- Microsoft **forces** you to connect to internet
- But then gives you a **secret command** to bypass their own rule!
- It's like Windows has a split personality:
  - **Front face**: "YOU MUST CONNECT! MICROSOFT ACCOUNT REQUIRED!" 😠
  - **Back door**: "Psst... here's how to skip everything we just demanded" 😉

**What It Actually Does**: 🔧
- Restarts the setup process fresh 🔄
- Magically makes the "I don't have internet" option appear ✨
- Lets you create a **local account** instead of Microsoft account 🏠
- Tells Windows: "Shush, let the human proceed without drama" 🤫

> **Pro Tip**: You've just used Microsoft's own tool to tame Microsoft's stubbornness! 🐧

## Complete Windows Setup 🏁
- **Username**: Set as 'bijoy' when asked 👤
- **Password**: Something like 'Windows-11' 🔐
- **Finish**: Installation completes and shows Desktop! 🖥️

## Post-Installation Cleanup 🧹
**Important**: Remove the windows11.iso device from VM configuration so next boot works properly! 📀❌

---

# 🎯 Installing VirtIO Drivers - The Performance Boost

## Step 1: Download VirtIO Magic ⬇️
In Ubuntu terminal:
```bash
wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso
```

**What's VirtIO?** 🤔
Paravirtualized drivers that make your VM run like lightning! ⚡

## Step 2: Attach Drivers to VM 📎
1. In Virtual Machine Manager, select your Windows VM
2. Click **"Open"** (not Run) to get VM details window 🪟
3. Click **"Add Hardware"** ➕
4. Select **"Storage"** 💾
5. **Device type**: CDROM device 📀
6. **Bus type**: SATA 🔌
7. **Storage format**: Browse → navigate to virtio-win.iso 📂
8. Click **Add** -Yes ✅
9. **Shut Down** & **Start**

## Step 3: Install the Drivers 🔧
1. Start your Windows VM 🚀
2. Open **File Explorer** in Windows 📁
3. Look for new CD drive (VirtIO drivers) 💿
4. Run **virtio-win-guest-tools.exe** 🏃‍♂️
5. Click through installer (accept defaults) ✅
6. Install everything and reboot 🔄

## Why Guest Tools is Amazing 🌟
The `virtio-win-guest-tools` installer is like having a **smart assistant** that:
- Detects your 64-bit Windows automatically 🔍
- Installs all the right drivers magically ✨
- Sets up clipboard sharing 📋
- Configures display drivers 🖥️
- Handles network drivers 🌐
- Does everything in one go! 🎯

## Step 4: Test Display Settings 🖥️
After reboot, in Windows:
1. Right-click desktop → **Display settings** ⚙️
2. Scroll to **Display resolution** 📐
3. Try different resolutions (you should see more options now!) 🎛️
4. Pick something like 1366x768 or whatever fits 📏

# Post-Driver Cleanup 🧹
Remove the virtio-win.iso device from VM configuration for clean future boots! 📀❌

> **⚠️ Important Note**: Always remove added hardware after use - VMs won't boot if they expect hardware that's not there!

# 🎉 Congratulations! You're Now a KVM Master!

You've just completed the **ultimate virtualization journey**! 🎊

**What you've accomplished**: 🏆
- ✅ Created a professional Windows 11 VM
- ✅ Optimized performance with VirtIO drivers

**You're now running**: 🎭
- A Windows computer inside your Linux computer
- Both thinking they're in charge
- With near-native performance (95-98%!)
- Using the same technology that powers the cloud! ☁️

---
