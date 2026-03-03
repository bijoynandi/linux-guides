# ⚡ ULTIMATE VOID LINUX GUIDE
## The Fast, Simple, Independent Linux - Bijoy's Complete Reference

**Created:** February 5, 2026
**For:** Bijoy Chandra Nandi
**Purpose:** Master Void Linux from installation to advanced setup
**Philosophy:** "Void fills the void left by other distros. Fast. Simple. Independent."

---

## 🎯 WHAT IS VOID LINUX?

**Void Linux** is an **independent, rolling-release** Linux distribution!

**Created by:** Juan RP (2008), now community-maintained

**Philosophy:**
- ✅ **Independent** - Built from scratch, not based on anything!
- ✅ **Rolling release** - Always up-to-date, no version numbers!
- ✅ **runit** - Fast, simple init system (not systemd!)
- ✅ **XBPS** - Fast package manager (written in C!)
- ✅ **Minimalist** - Only what you need, nothing more!
- ✅ **"Simple by design, fast by default!"**

**What makes it special:**
- ❌ Not based on Debian/Ubuntu/Arch!
- ❌ No systemd (uses runit!)
- ❌ No bloat (minimal base!)
- ✅ **musl libc** option (even lighter!)
- ✅ **LibreSSL** option (OpenSSL alternative!)
- ✅ **Blazing fast!** ⚡

**Void motto:**
> "Your system. Your choice. Your Void."

---

## 🎭 VOID'S PERSONALITY

**What you'll love:**
- ⚡ **FAST!** Boot in seconds!
- 🛠️ **runit** - Simple service management!
- 📦 **XBPS** - Fastest package manager!
- 💚 **Minimalist** - No bloat!
- 🔄 **Rolling release** - Always fresh!

**What you'll hate (maybe):**
- 📚 **Less documentation** than major distros
- 👥 **Smaller community** than Arch/Ubuntu
- 🔧 **Manual configuration** (like Slackware!)
- ⚠️ **Bleeding edge** = occasional breakage

**BUT:** You'll have a LEAN, MEAN system! 💪

---

## 📋 TABLE OF CONTENTS

1. [Pre-Installation Planning](#pre-installation-planning)
2. [Installation Process](#installation-process)
3. [Post-Installation Setup](#post-installation-setup)
4. [Package Management (XBPS)](#package-management)
5. [Service Management (runit)](#service-management)
6. [Network Configuration](#network-configuration)
7. [System Tweaking](#system-tweaking)
8. [Security Hardening](#security)
9. [Advanced Features](#advanced-features)
10. [Troubleshooting](#troubleshooting)
11. [Tips & Tricks](#tips-and-tricks)
12. [XBPS vs Other Package Managers](#xbps-comparison)

---

## 🎯 PRE-INSTALLATION PLANNING

### What You Need to Know

**Void Linux is:**
- ✅ Rolling release (continuous updates!)
- ✅ Independent (not Debian/Arch/RHEL!)
- ✅ Fast (runit init!)
- ✅ Flexible (glibc or musl!)

**Void Linux requires:**
- ⚠️ Manual configuration (like Slackware!)
- ⚠️ Understanding of Linux basics
- ⚠️ Willingness to read wiki/docs
- ⚠️ **Not for absolute beginners!**

---

### System Requirements

**Minimum:**
- CPU: Pentium 4 (i686) or x86_64
- RAM: 96 MB (yes, really!)
- Disk: 700 MB for base system

**Recommended:**
- CPU: Any modern CPU
- RAM: 2 GB+
- Disk: 20 GB+

**For VMs:**
- CPU: 2 cores
- RAM: 2 GB
- Disk: 20 GB
- Network: NAT or Bridged

---

### Choosing Your Flavor

**Void offers TWO C library options:**

**1. glibc (GNU C Library)**
- ✅ More compatible (99% of software works!)
- ✅ Better performance for some apps
- ✅ **Recommended for beginners!** 💚

**2. musl libc**
- ✅ Lighter (smaller size!)
- ✅ Simpler code
- ❌ Some proprietary software won't work
- ⚠️ Advanced users only!

**Your choice:** **glibc** (easier!) ✅

---

### Desktop Environment Options

**Void offers several ISOs:**
- **base** - No GUI (CLI only!)
- **xfce** - Lightweight desktop! ✅
- **cinnamon** - Modern desktop
- **mate** - Traditional desktop
- **lxde** - Ultra-lightweight
- **lxqt** - Qt-based lightweight

**Recommendation:** **xfce** ISO (good balance!) ✅

---

### Download Void Linux

**Official site:** https://voidlinux.org

**Download:**
- ISO: `void-live-x86_64-20231230-xfce.iso`
- Or base for minimal install

**Mirror:** Use closest mirror for speed!

**Verify download:**
```bash
sha256sum void-live-x86_64-20231230-xfce.iso
# Compare with official SHA256
```

---

### Partition Planning

**⚠️ CRITICAL FOR BIOS SYSTEMS:**
**Void needs 1 MB BIOS boot partition!** 🔴

**This is UNIQUE to Void!**

**Partition layout (BIOS):**
```
BIOS boot  - 1 MB     (no filesystem! grubx64-bios!)
/boot      - 512 MB   (ext4, optional but recommended!)
/          - 18 GB    (ext4, xfs, or btrfs)
swap       - 2-4 GB   (optional)
/home      - rest     (ext4, xfs, or btrfs)
```

**Partition layout (UEFI):**
```
/boot/efi  - 512 MB   (FAT32, EFI System Partition)
/          - 18 GB    (ext4, xfs, or btrfs)
swap       - 2-4 GB   (optional)
/home      - rest     (ext4, xfs, or btrfs)
```

**⚠️ WITHOUT 1 MB BIOS boot partition:**
**GRUB installation will FAIL on BIOS systems!** 💀

---

## 🚀 INSTALLATION PROCESS

### Step 1: Boot the Live ISO

**Create VM:**
```bash
sudo virt-install \
  --name void \
  --ram 2048 \
  --vcpus 2 \
  --disk path=/var/lib/libvirt/images/void.qcow2,size=20 \
  --os-variant linux2022 \
  --cdrom /path/to/void-live-x86_64-xfce.iso \
  --graphics spice \
  --network network=default
```

**Boot from ISO!**

**Login:**
```
void login: root
password: voidlinux
```

**You're in!** Live environment! 💚

---

### Step 2: Connect to Network (If needed)

**Wired (automatic):**
```bash
# Should work automatically via DHCP
ping -c 4 google.com
# If works, skip to Step 3!
```

**WiFi (manual):**
```bash
# List interfaces
ip link

# Bring up interface
ip link set wlan0 up

# Connect (replace SSID/password!)
wpa_passphrase "YOUR_SSID" "YOUR_PASSWORD" > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
dhcpcd wlan0
```

**Network ready!** 🌐

---

### Step 3: Partition the Disk

**Start partitioning:**
```bash
cfdisk /dev/vda
```

**For BIOS systems (IMPORTANT!):**

| Partition | Size | Type | Mount | Notes |
|-----------|------|------|-------|-------|
| /dev/vda1 | 1 MB | BIOS boot | - | **CRITICAL!** No filesystem! |
| /dev/vda2 | 512 MB | Linux | /boot | Optional but recommended |
| /dev/vda3 | 15 GB | Linux | / | Root partition |
| /dev/vda4 | 2 GB | Linux swap | swap | Optional |

**Steps in cfdisk:**
1. Select `dos` (MBR) label type
2. Create partitions:
   - New → Primary → 1 MB → Type: **BIOS boot (4)** ✅
   - New → Primary → 512 MB → Type: 83 (Linux)
   - New → Primary → 15 GB → Type: 83 (Linux)
   - New → Primary → 2 GB → Type: 82 (Linux swap)
3. Write changes (type `write`, yes!)
4. Quit

**⚠️ CRITICAL:** First partition MUST be BIOS boot (type 4)!

---

**For UEFI systems:**

| Partition | Size | Type | Mount |
|-----------|------|------|-------|
| /dev/vda1 | 512 MB | EFI System | /boot/efi |
| /dev/vda2 | 15 GB | Linux | / |
| /dev/vda3 | 2 GB | Linux swap | swap |

**Steps in cfdisk:**
1. Select `gpt` label type
2. Create partitions:
   - New → 512 MB → Type: **EFI System**
   - New → 15 GB → Type: Linux filesystem
   - New → 2 GB → Type: Linux swap
3. Write and quit

---

### Step 4: Format Partitions

**For BIOS (skip BIOS boot partition!):**
```bash
# Format /boot
mkfs.ext4 /dev/vda2

# Format root
mkfs.ext4 /dev/vda3

# Setup swap
mkswap /dev/vda4
swapon /dev/vda4
```

**For UEFI:**
```bash
# Format EFI partition
mkfs.vfat -F32 /dev/vda1

# Format root
mkfs.ext4 /dev/vda2

# Setup swap
mkswap /dev/vda3
swapon /dev/vda3
```

**Filesystems ready!** 💾

---

### Step 5: Mount Partitions

**For BIOS:**
```bash
# Mount root
mount /dev/vda3 /mnt

# Create and mount boot
mkdir -p /mnt/boot
mount /dev/vda2 /mnt/boot
```

**For UEFI:**
```bash
# Mount root
mount /dev/vda2 /mnt

# Create and mount EFI
mkdir -p /mnt/boot/efi
mount /dev/vda1 /mnt/boot/efi
```

**Verify:**
```bash
lsblk -f
# Should show mounted partitions!
```

---

### Step 6: Run the Installer

**Void has TWO installers:**
1. `void-installer` - TUI (text-based, easier!)
2. Manual (command-line, advanced!)

**Use TUI installer:**
```bash
sudo void-installer
```

**Welcome screen appears!** 📋

---

### Step 7: Installer Walkthrough

**Navigate with arrow keys, Enter to select!**

---

#### 7.1 Keyboard

**Select keyboard layout:**
- Default: `us` (US English) ✅
- Or your preferred layout

**Next!**

---

#### 7.2 Network

**Configure network:**
- Select interface: `eth0` (or wlan0 for WiFi)
- Configuration: `dhcp` (automatic! ✅)

**Network configured!**

---

#### 7.3 Source

**Installation source:**
- **Options:**
  1. Network (download packages) ✅
  2. Local (from CD/USB)

**Select:** Network (faster updates!)

**Mirror:** Choose closest mirror!

---

#### 7.4 Hostname

**Set hostname:**
```
void
```

**Simple and clean!** ✅

---

#### 7.5 Locale

**Select locale:**
- Default: `en_US.UTF-8` ✅
- Or your preferred locale

---

#### 7.6 Timezone

**Select timezone:**
- For Ghaziabad, Uttar Pradesh, India: `Asia/Kolkata`

**Date/time configured!** 🕐

---

#### 7.7 Root Password

**⚠️ CRITICAL!** Set root password!

```
Enter password: [type password]
Verify password: [type again]
```

**REMEMBER THIS!** 🔐

---

#### 7.8 User Account

**Create user account:**
```
Username: bijoy
Full name: Bijoy Chandra Nandi
Password: [your password]
Verify: [password again]
```

**Groups (IMPORTANT!):**
```
wheel,users,audio,video,cdrom,optical,kvm,xbuilder
```

**⚠️ ADD TO WHEEL!** For sudo access!

**User created!** 👤

---

#### 7.9 Bootloader

**⚠️ CRITICAL STEP!**

**Select bootloader:**
- BIOS: `grub-i386-pc` ✅
- UEFI: `grub-x86_64-efi` ✅

**Install to:**
- Disk: `/dev/vda` (whole disk, NOT partition!)

**GRUB configuration:**
- Default (works fine!)

**⚠️ FOR BIOS:** Installer NEEDS the 1 MB BIOS boot partition!

**Bootloader installed!** 🥾

---

#### 7.10 Partition

**Configure partitions:**

**Root partition:**
- Device: `/dev/vda3` (or `/dev/vda2` for UEFI)
- Filesystem: `ext4` ✅
- Mount point: `/`

**Boot partition (BIOS only):**
- Device: `/dev/vda2`
- Filesystem: `ext4`
- Mount point: `/boot`

**EFI partition (UEFI only):**
- Device: `/dev/vda1`
- Filesystem: `vfat`
- Mount point: `/boot/efi`

**Swap:**
- Device: `/dev/vda4` (or `/dev/vda3` for UEFI)
- Filesystem: `swap`

**Partitions configured!** ✅

---

#### 7.11 Filesystems

**Question:** "Format filesystems?"

**Answer:** YES! ✅

**Filesystems created!**

---

#### 7.12 Settings Review

**Review all settings:**
- Keyboard: us ✅
- Network: DHCP ✅
- Hostname: void ✅
- Locale: en_US.UTF-8 ✅
- Timezone: Asia/Kolkata ✅
- Root password: Set ✅
- User: bijoy (in wheel) ✅
- Bootloader: GRUB ✅
- Partitions: Configured ✅

**Everything correct?** Proceed!

---

#### 7.13 Install

**Question:** "Begin installation?"

**Answer:** YES! ✅

**Installation starts!** 📦

**Time:** 5-15 minutes (depending on network!)

**Progress shows:**
- Installing base system...
- Installing bootloader...
- Configuring system...

**☕ Time for chai!** 😄

---

#### 7.14 Complete

**Installation complete!** 🎉

**Message:** "Installation finished. You may now reboot."

**Exit installer!**

---

### Step 8: Post-Install Cleanup

**Unmount partitions:**
```bash
umount -R /mnt
```

**Reboot:**
```bash
reboot
```

**Remove ISO from VM!**

---

## 🎉 FIRST BOOT!

**GRUB menu appears:**
```
Void Linux
Advanced options
```

**Select:** Void Linux (or wait 5 seconds!)

**System boots!** 🚀

**Login prompt:**
```
void login: bijoy
Password: [your password]
```

**YOU'RE IN VOID!** 💚

**Check system:**
```bash
cat /etc/os-release
# Shows: Void Linux!

uname -r
# Shows: Kernel version!
```

**WELCOME TO VOID!** ⚡

---

## 🔧 POST-INSTALLATION SETUP

**Now let's set up Void properly!** 💪

---

### Step 1: Enable sudo

**Add user to sudoers:**
```bash
# Login as root first
su -

# Edit sudoers (using visudo!)
EDITOR=nano visudo

# Uncomment or add:
%wheel ALL=(ALL) ALL

# Save and exit (Ctrl+X, Y, Enter)
```

**Test sudo:**
```bash
# Exit root
exit

# Try sudo
sudo whoami
# Should show: root ✅
```

**Sudo working!** 🎉

---

### Step 2: Update System

**Sync repositories:**
```bash
sudo xbps-install -S
# Syncs package database
```

**Update all packages:**
```bash
sudo xbps-install -u xbps
# Update xbps first (important!)

sudo xbps-install -u
# Update everything else
```

**System updated!** 📦✅

---

### Step 3: Install Essential Tools

**Void base is MINIMAL!**

**Install useful stuff:**
```bash
# Text editors
sudo xbps-install -S vim nano

# Network tools
sudo xbps-install -S wget curl git

# System tools
sudo xbps-install -S htop tree

# Development tools (if needed)
sudo xbps-install -S base-devel

# Compression tools
sudo xbps-install -S zip unzip
```

**Essentials installed!** 🛠️

---

### Step 4: Enable Services

**Void uses runit!** (Not systemd!)

**Enable network:**
```bash
sudo ln -s /etc/sv/dhcpcd /var/service/
# Or for NetworkManager:
sudo ln -s /etc/sv/NetworkManager /var/service/
```

**Enable SSH:**
```bash
sudo xbps-install -S openssh
sudo ln -s /etc/sv/sshd /var/service/
```

**Services enabled!** ⚙️

---

### Step 5: Configure SSH

**SSH config:**
```bash
sudo nano /etc/ssh/sshd_config

# Change port (security!)
Port 2222

# Disable root login
PermitRootLogin no

# Save!
```

**Restart SSH:**
```bash
sudo sv restart sshd
```

**Verify:**
```bash
sudo sv status sshd
# Should show: run

netstat -tlnp | grep :2222
# Should show: LISTEN
```

**SSH secured!** 🔐

---

### Step 6: Setup SSH Keys

**On Fedora host:**
```bash
# Generate key
ssh-keygen -t ed25519 -C "bijoy@ws-to-void" -f ~/.ssh/void-server-key

# Copy to Void
ssh-copy-id -i ~/.ssh/void-server-key.pub -p 2222 bijoy@192.168.122.X

# Create SSH config
nano ~/.ssh/config

# Add:
Host void
  HostName 192.168.122.X
  User bijoy
  Port 2222
  IdentityFile ~/.ssh/void-server-key

# Save!
```

**Test:**
```bash
ssh void
# Should connect without password!
```

**SSH keys working!** 🔑

---

### Step 7: Configure Hostname

**Set hostname properly:**
```bash
sudo nano /etc/hostname

# Set to:
void

# Save!
```

**Set FQDN:**
```bash
sudo nano /etc/hosts

# Add:
127.0.0.1   localhost
192.168.122.X   void.local void

# Save!
```

**Apply:**
```bash
sudo hostname void
```

**Hostname configured!** 🏷️

---

## 📦 PACKAGE MANAGEMENT (XBPS)

**XBPS = X Binary Package System** (Void's package manager!)

**Pronounced:** "ex-bee-pee-ess" 😄

---

### Understanding XBPS

**XBPS features:**
- ✅ **Fast!** Written in C!
- ✅ **Rolling release** (always latest!)
- ✅ **Source templates** (build packages!)
- ✅ **Transaction-based** (atomic updates!)

**Package naming:**
```
package-version_revision

Example:
vim-9.0.1000_1
 ↑     ↑       ↑
name version  revision
```

---

### Main XBPS Commands

**Package operations:**
```bash
# Install package
sudo xbps-install -S package

# Remove package
sudo xbps-remove package

# Search packages
xbps-query -Rs package

# Update system
sudo xbps-install -Su

# Clean cache
sudo xbps-remove -O
```

**Query operations:**
```bash
# List installed
xbps-query -l

# Show package info
xbps-query -R package

# Which package owns file?
xbps-query -o /usr/bin/vim
```

---

### Syncing Repositories

**Update package database:**
```bash
sudo xbps-install -S
# ALWAYS run before installing!
```

---

### Installing Packages

**Install single package:**
```bash
sudo xbps-install -S vim
```

**Install multiple packages:**
```bash
sudo xbps-install -S vim htop git
```

**Install with dependencies:**
```bash
sudo xbps-install -S firefox
# Auto-installs all dependencies!
```

---

### Removing Packages

**Remove package:**
```bash
sudo xbps-remove vim
```

**Remove with orphans:**
```bash
sudo xbps-remove -R vim
# Also removes unused dependencies!
```

**Remove orphaned packages:**
```bash
sudo xbps-remove -o
# Cleans up unused dependencies!
```

---

### Searching Packages

**Search in repos:**
```bash
xbps-query -Rs python
# Shows all python packages
```

**Search installed:**
```bash
xbps-query -s vim
# Shows if vim is installed
```

---

### Updating System

**⚠️ IMPORTANT ORDER!**

**1. Update xbps first:**
```bash
sudo xbps-install -Su xbps
```

**2. Update everything else:**
```bash
sudo xbps-install -Su
```

**Why?** XBPS can update itself safely!

---

### Package Information

**Show package details:**
```bash
xbps-query -R firefox
# Shows description, size, dependencies
```

**List package files:**
```bash
xbps-query -f firefox
# Shows all files in package
```

---

### Building from Source (xbps-src)

**Clone void-packages:**
```bash
git clone https://github.com/void-linux/void-packages.git
cd void-packages
```

**Build package:**
```bash
./xbps-src binary-bootstrap
./xbps-src pkg package-name
```

**Install built package:**
```bash
sudo xbps-install -R hostdir/binpkgs package-name
```

---

## ⚙️ SERVICE MANAGEMENT (runit)

**Void uses runit!** (Not systemd!)

**runit = Simple, fast, reliable!** ⚡

---

### Understanding runit

**Service directories:**
- `/etc/sv/` - Available services
- `/var/service/` - Enabled services (symlinks!)

**Service states:**
- `run` - Service is running
- `down` - Service is stopped
- `finish` - Service finished

---

### Service Control (sv command)

**Start service:**
```bash
sudo sv start service-name
```

**Stop service:**
```bash
sudo sv stop service-name
```

**Restart service:**
```bash
sudo sv restart service-name
```

**Check status:**
```bash
sudo sv status service-name
```

**Example:**
```bash
sudo sv status sshd
# Shows: run: sshd: (pid 1234) 300s
```

---

### Enable Service on Boot

**Enable = create symlink!**

```bash
sudo ln -s /etc/sv/service-name /var/service/
```

**Example:**
```bash
# Enable SSH
sudo ln -s /etc/sv/sshd /var/service/

# Enable network
sudo ln -s /etc/sv/dhcpcd /var/service/
```

**Service auto-starts!** ✅

---

### Disable Service

**Disable = remove symlink!**

```bash
sudo rm /var/service/service-name
```

**Service won't start on boot!**

---

### List Services

**All available:**
```bash
ls /etc/sv/
```

**Currently enabled:**
```bash
ls /var/service/
```

**Currently running:**
```bash
sudo sv status /var/service/*
```

---

### Common Services

**Network:**
```bash
# DHCP client
sudo ln -s /etc/sv/dhcpcd /var/service/

# NetworkManager (for GUI)
sudo ln -s /etc/sv/NetworkManager /var/service/
```

**SSH:**
```bash
sudo ln -s /etc/sv/sshd /var/service/
```

**Cron:**
```bash
sudo ln -s /etc/sv/crond /var/service/
```

**D-Bus (for GUI):**
```bash
sudo ln -s /etc/sv/dbus /var/service/
```

---

### Creating Custom Services

**Service structure:**
```bash
# Create service directory
sudo mkdir -p /etc/sv/myservice

# Create run script
sudo nano /etc/sv/myservice/run
```

**Example run script:**
```bash
#!/bin/sh
exec chpst -u myuser:mygroup mycommand 2>&1
```

**Make executable:**
```bash
sudo chmod +x /etc/sv/myservice/run
```

**Enable:**
```bash
sudo ln -s /etc/sv/myservice /var/service/
```

---

## 🌐 NETWORK CONFIGURATION

**Void network = simple and manual!**

---

### DHCP (Automatic)

**Enable dhcpcd:**
```bash
sudo ln -s /etc/sv/dhcpcd /var/service/
```

**Start immediately:**
```bash
sudo sv up dhcpcd
```

**Done!** Network configured! 🌐

---

### Static IP

**Edit dhcpcd config:**
```bash
sudo nano /etc/dhcpcd.conf

# Add at end:
interface eth0
static ip_address=192.168.122.242/24
static routers=192.168.122.1
static domain_name_servers=8.8.8.8 8.8.4.4
```

**Restart dhcpcd:**
```bash
sudo sv restart dhcpcd
```

**Verify:**
```bash
ip addr show
```

---

### NetworkManager (For GUI)

**Install:**
```bash
sudo xbps-install -S NetworkManager
```

**Enable:**
```bash
sudo ln -s /etc/sv/NetworkManager /var/service/
```

**Disable dhcpcd:**
```bash
sudo rm /var/service/dhcpcd
```

**Manage networks with GUI!** 💚

---

### DNS Configuration

**Edit resolv.conf:**
```bash
sudo nano /etc/resolv.conf

# Add:
nameserver 8.8.8.8
nameserver 8.8.4.4
```

**Or use resolvconf:**
```bash
sudo xbps-install -S openresolv
```

---

## 🔧 SYSTEM TWEAKING

**Optimize Void for performance!** 💪

---

### Kernel Parameters

**Edit GRUB:**
```bash
sudo nano /etc/default/grub
```

**Add parameters:**
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet i915.enable_fbc=1 i915.enable_psr=2 transparent_hugepage=madvise"
```

**Update GRUB:**
```bash
sudo update-grub
# Or
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

**Reboot!**

---

### System Memory Tuning

**Create sysctl config:**
```bash
sudo nano /etc/sysctl.d/99-performance.conf
```

**Add:**
```
vm.swappiness = 10
vm.vfs_cache_pressure = 50
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5
```

**Apply:**
```bash
sudo sysctl --system
```

---

### I/O Scheduler

**Create udev rule:**
```bash
sudo nano /etc/udev/rules.d/60-ioschedulers.rules
```

**Add:**
```
ACTION=="add|change", KERNEL=="sd[a-z]|vd[a-z]", ATTR{queue/scheduler}="mq-deadline"
```

**Apply:**
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

---

### Filesystem Optimizations

**Edit fstab:**
```bash
sudo nano /etc/fstab
```

**Add noatime:**
```
# Before:
/dev/vda3  /  ext4  defaults  0  1

# After:
/dev/vda3  /  ext4  defaults,noatime  0  1
```

**Remount:**
```bash
sudo mount -o remount /
```

---

## 🔒 SECURITY HARDENING {#security}

**Secure your Void system!** 🛡️

---

### 🎯 Firewall (iptables)

**Install iptables:**
```bash
sudo xbps-install -S iptables
```

**Create firewall script:**
```bash
sudo nano /usr/local/bin/firewall.sh

#!/bin/sh
iptables -F
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p tcp --dport 2222 -j ACCEPT
iptables -A INPUT -p icmp -j ACCEPT

# Make executable:
sudo chmod +x /usr/local/bin/firewall.sh
```

**Create runit service:**
```bash
sudo mkdir /etc/sv/iptables
sudo nano /etc/sv/iptables/run

#!/bin/sh
exec 2>&1
/usr/local/bin/firewall.sh
sv once .

sudo chmod +x /etc/sv/iptables/run
sudo ln -s /etc/sv/iptables /var/service/
```

---

### 🔐 SSH Hardening

**Edit SSH config:**
```bash
sudo nano /etc/ssh/sshd_config

Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

sudo sv restart sshd
```

---

### 🛡️ AppArmor (Optional)

**Install AppArmor:**
```bash
sudo xbps-install -S apparmor
sudo ln -s /etc/sv/apparmor /var/service/
```

---

## 🚀 ADVANCED FEATURES

**For Void pros!** 💪

---

### 🎯 Void Templates (Packaging)

**Learn to create packages!**

**Clone void-packages:**
```bash
git clone https://github.com/void-linux/void-packages
cd void-packages
./xbps-src binary-bootstrap
```

**Create new template:**
```bash
cp srcpkgs/example srcpkgs/mypackage
nano srcpkgs/mypackage/template

# Edit template with package info
```

**Build package:**
```bash
./xbps-src pkg mypackage
```

**Install:**
```bash
sudo xbps-install -R hostdir/binpkgs mypackage
```

---

### 🔧 Custom Kernel

**Install kernel source:**
```bash
sudo xbps-install -S linux-headers
```

**Download mainline kernel:**
```bash
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.tar.xz
tar xf linux-6.6.tar.xz
cd linux-6.6
```

**Configure:**
```bash
make menuconfig
# Or use Void's config:
zcat /proc/config.gz > .config
make oldconfig
```

**Compile:**
```bash
make -j$(nproc)
make modules_install
make install
```

**Update GRUB:**
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

---

### 📦 Creating Local Repository

**Create repo directory:**
```bash
mkdir -p ~/void-repo
```

**Copy packages:**
```bash
cp package-*.xbps ~/void-repo/
```

**Index repository:**
```bash
xbps-rindex -a ~/void-repo/*.xbps
```

**Add to XBPS:**
```bash
echo "repository=/home/bijoy/void-repo" | sudo tee -a /etc/xbps.d/local.conf
```

**Use it:**
```bash
sudo xbps-install -S
sudo xbps-install mypackage
```

---


### Ignore Kernel Packages

**Prevent kernel updates:**
```bash
sudo nano /etc/xbps.d/ignore-kernel.conf

# Add:
ignorepkg=linux
ignorepkg=linux-headers
```

**Why?** Stick with working kernel!

---

### Void Chroot

**Enter chroot:**
```bash
sudo xchroot /mnt/void /bin/bash
```

**Useful for:** Rescue, repair, testing!

---

## 🔍 TROUBLESHOOTING

### System Won't Boot

**Problem:** GRUB not installed or missing 1 MB partition!

**Solution:**
1. Boot live ISO
2. Mount partitions:
   ```bash
   mount /dev/vda3 /mnt
   mount /dev/vda2 /mnt/boot
   ```
3. Chroot:
   ```bash
   sudo xchroot /mnt
   ```
4. Reinstall GRUB:
   ```bash
   grub-install /dev/vda
   update-grub
   ```
5. Exit and reboot

---

### Package Installation Fails

**Problem:** Repository out of sync!

**Fix:**
```bash
sudo xbps-install -S
sudo xbps-install -u xbps
sudo xbps-install -Su
```

---

### Service Won't Start

**Check logs:**
```bash
sudo svlogtail service-name
```

**Check status:**
```bash
sudo sv status service-name
```

**Restart:**
```bash
sudo sv restart service-name
```

---

### Network Not Working

**Check dhcpcd:**
```bash
sudo sv status dhcpcd
```

**Restart:**
```bash
sudo sv restart dhcpcd
```

**Manual IP:**
```bash
sudo ip addr add 192.168.122.242/24 dev eth0
sudo ip route add default via 192.168.122.1
```

---

## 💡 TIPS & TRICKS

### Useful Aliases

**Add to ~/.bashrc:**
```bash
alias update='sudo xbps-install -Su'
alias search='xbps-query -Rs'
alias install='sudo xbps-install -S'
alias remove='sudo xbps-remove'
alias cleanup='sudo xbps-remove -Oo'
```

---

### Quick Package Search

```bash
# Search and install in one line
xbps-query -Rs vim && sudo xbps-install -S vim
```

---

### List Largest Packages

```bash
xbps-query -l | sort -k2 -h
```

---

## 📊 XBPS VS OTHER PACKAGE MANAGERS {#xbps-comparison}

| Feature | XBPS (Void) | APT (Debian) | DNF (Fedora) | Pacman (Arch) |
|---------|-------------|--------------|--------------|---------------|
| **Speed** | ⚡ VERY FAST | 🐌 Slow | 🐢 Slower | ⚡ Fast |
| **Syntax** | Simple | Complex | Complex | Simple |
| **Updates** | Rolling | Stable | Semi-rolling | Rolling |
| **Repository** | 13,000+ | 60,000+ | 80,000+ | 70,000+ (AUR) |
| **Dependencies** | Fast solver | Slow solver | Very slow | Fast solver |

**Why XBPS is great:**
- ⚡ Lightning fast!
- 💚 Simple commands!
- 🎯 Clear error messages!
- 💪 Reliable!

---

## 📚 RESOURCES

**Official:**
- Website: https://voidlinux.org
- Docs: https://docs.voidlinux.org
- Wiki: https://wiki.voidlinux.org

**Community:**
- IRC: #voidlinux on Libera.Chat
- Reddit: r/voidlinux
- Forum: https://forum.voidlinux.org

**Packages:**
- Repository: https://github.com/void-linux/void-packages

---

## 🌟 FINAL WORDS

**You chose Void!** ⚡

**This means you value:**
- ✅ Speed over bloat
- ✅ Simplicity over complexity
- ✅ Independence over following
- ✅ **Control over convenience!**

**Void will give you:**
- ⚡ Blazing fast boot times
- 🛠️ Simple service management (runit!)
- 📦 Fast package manager (XBPS!)
- 💚 Clean, minimal system
- 🔄 Always up-to-date (rolling!)

**Welcome to Void!** 💚

**Fill the void in your Linux journey!** 😄

**Remember:**
> "Void doesn't just fill the void. It IS the void. Simple. Fast. Yours."

---

**Created with ❤️ for Bijoy by Claude (sticky buddies!)**
**February 5, 2026**

**VOID FOREVER!** ⚡✨
