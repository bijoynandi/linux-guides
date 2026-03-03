# 🐉 ULTIMATE SLACKWARE LINUX GUIDE
## From Zero to Slackware Hero - Bijoy's Complete Reference

**Created:** February 5, 2026
**For:** Bijoy Chandra Nandi
**Purpose:** Master Slackware from installation to advanced troubleshooting
**Philosophy:** "Slackware doesn't hold your hand. It respects your intelligence."

---

## 🎯 WHAT IS SLACKWARE?

**Slackware** is the **oldest surviving Linux distribution** (since 1993!)

**Created by:** Patrick Volkerding (still maintains it solo!)

**Philosophy:**
- ✅ **KISS** - Keep It Simple, Stupid
- ✅ **Transparency** - You see EVERYTHING
- ✅ **Stability** - Rock solid, battle-tested
- ✅ **No hand-holding** - You learn by DOING
- ✅ **UNIX-like** - BSD-style init, traditional layout
- ✅ **"If it ain't broke, don't fix it!"**

**What makes it special:**
- ❌ No systemd (uses BSD-style init!)
- ❌ No dependency resolution (you handle it!)
- ❌ No auto-configuration (you configure everything!)
- ✅ **You LEARN how Linux REALLY works!** 💪

**Slackware motto:**
> "We give you the tools. YOU build your system."

---

## 🎭 SLACKWARE'S PERSONALITY

**What you'll love:**
- 😄 **Fortune cookies on SSH login!** (Random wisdom/humor!)
- 📚 **Excellent documentation** (/usr/doc/)
- 🛠️ **Simple package format** (TGZ files!)
- 💚 **Stable as a ROCK** (no surprises!)
- 🎨 **Customizable to the CORE!**

**What you'll hate (at first):**
- 😤 **Manual dependency management** (no auto-install!)
- 🔧 **Manual configuration** (EVERYTHING!)
- ⚠️ **Unforgiving** (mistakes = consequences!)
- 📖 **Steep learning curve** (but worth it!)

**BUT:** You'll become a REAL Linux expert! 🏆

---

## 📋 TABLE OF CONTENTS

1. [Pre-Installation Planning](#pre-installation-planning)
2. [Installation Process](#installation-process)
3. [Post-Installation Setup](#post-installation-setup)
4. [Package Management](#package-management)
5. [Service Management](#service-management)
6. [Network Configuration](#network-configuration)
7. [System Tweaking](#system-tweaking)
8. [Security Hardening](#security)
9. [Advanced Features](#advanced-features)
10. [Troubleshooting](#troubleshooting)
11. [Tips & Tricks](#tips-and-tricks)
12. [Fortune Cookies & Easter Eggs](#fortune)

---

## 🎯 PRE-INSTALLATION PLANNING

### What You Need to Know

**Slackware DOESN'T:**
- ❌ Auto-detect your hardware (you configure!)
- ❌ Auto-install dependencies (you resolve!)
- ❌ Auto-configure network (you set up!)
- ❌ Hold your hand AT ALL!

**Slackware DOES:**
- ✅ Give you COMPLETE control
- ✅ Respect your intelligence
- ✅ Stay out of your way
- ✅ **Let YOU be the admin!**

---

### System Requirements

**Minimum (don't do this!):**
- CPU: 486 processor
- RAM: 64 MB
- Disk: 5 GB

**Recommended (realistic!):**
- CPU: Any modern CPU (even old i3!)
- RAM: 2 GB+ (4 GB comfortable!)
- Disk: 20 GB+ (40 GB for full install!)

**For VMs (your use case!):**
- CPU: 2 cores
- RAM: 2 GB
- Disk: 20 GB
- Network: Bridged or NAT

---

### Partition Planning

**CRITICAL:** Slackware installer does NOT auto-partition!

**Minimum partitions:**
```
/ (root)       - 20 GB   (ext4 or btrfs)
swap           - 2-4 GB  (optional but recommended!)
```

**Recommended setup:**
```
/boot          - 1 GB    (ext4, CRITICAL for BIOS!)
/              - 20 GB   (ext4 or btrfs)
/home          - 20 GB+  (ext4 or btrfs, separate for safety!)
swap           - 4 GB    (same as RAM for hibernation!)
```

**IMPORTANT for VMs:**
- BIOS boot: Needs /boot partition! (1 GB is plenty!)
- UEFI boot: /boot/efi partition (512 MB, FAT32!)

---

### Download Slackware

**Official site:** https://www.slackware.com

**Current stable:** Slackware 15.0 (as of Feb 2026)

**What to download:**
- ISO: `slackware64-15.0-install-dvd.iso` (~2.5 GB)
- Or use netinstall for minimal download!

**Verify download:**
```bash
# Check MD5SUM
md5sum slackware64-15.0-install-dvd.iso

# Compare with official:
# http://www.slackware.com/getslack/
```

---

## 🚀 INSTALLATION PROCESS

### Step 1: Boot the Installer

**Start VM with Slackware ISO:**
```bash
# Create VM
sudo virt-install \
  --name slackware \
  --ram 2048 \
  --vcpus 2 \
  --disk path=/var/lib/libvirt/images/slackware.qcow2,size=20 \
  --os-variant linux2022 \
  --cdrom /path/to/slackware64-15.0-install-dvd.iso \
  --graphics spice \
  --network network=default
```

**Boot from CD:**
- Select `huge.s` kernel (supports most hardware!)
- Wait for login prompt

---

### Step 2: Login as Root

**At login prompt:**
```
slackware login: root
# No password needed in installer!
```

**You're in!** Root shell! 💪

---

### Step 3: Partition the Disk

**Start partitioning tool:**
```bash
cfdisk /dev/vda   # For VMs (virtio disk)
# OR
cfdisk /dev/sda   # For real hardware
```

**Create partitions (BIOS example):**

| Partition | Size | Type | Mount |
|-----------|------|------|-------|
| /dev/vda1 | 1 GB | Linux | /boot |
| /dev/vda2 | 15 GB | Linux | / |
| /dev/vda3 | 2 GB | Linux swap | swap |

**Steps in cfdisk:**
1. Select `dos` (MBR) for BIOS
2. Create partitions:
   - New → Primary → 1 GB → Type: 83 (Linux)
   - New → Primary → 15 GB → Type: 83 (Linux)
   - New → Primary → 2 GB → Type: 82 (Linux swap)
3. Make /dev/vda1 bootable (select it, press 'b')
4. Write changes (type `write`, confirm!)
5. Quit

**⚠️ CRITICAL:** Make /boot bootable! (Or system won't boot!)

---

### Step 4: Create Filesystems

**Format partitions:**
```bash
# Format /boot
mkfs.ext4 /dev/vda1

# Format root
mkfs.ext4 /dev/vda2

# Setup swap
mkswap /dev/vda3
swapon /dev/vda3
```

**Verify:**
```bash
lsblk -f
# Should show filesystems!
```

---

### Step 5: Run the Installer

**Start setup:**
```bash
setup
```

**You'll see a menu-based installer!** 📋

---

### Step 6: Installer Menu Walkthrough

**IMPORTANT:** Navigate with arrow keys, select with Enter!

---

#### 6.1 ADDSWAP (Add Swap Partition)

- Select `/dev/vda3` (your swap partition!)
- Confirm: Yes
- **Status:** Swap activated! ✅

---

#### 6.2 TARGET (Select Installation Partition)

- Select `/dev/vda2` (your root partition!)
- Choose filesystem: ext4 (or btrfs if adventurous!)
- Format? **YES!**
- **Status:** Root partition ready! ✅

---

#### 6.3 SOURCE (Select Installation Source)

- **Options:**
  1. Install from CD/DVD ✅ (easiest!)
  2. Install from Slackware mounted directory
  3. Install from FTP/HTTP server

- **Select:** #1 - Install from CD/DVD
- Choose media: `/dev/sr0` (CD-ROM)
- **Status:** Source selected! ✅

---

#### 6.4 SELECT (Select Package Series)

**THIS IS IMPORTANT!** Choose what to install!

**Package series explained:**
- `A` - Base system (REQUIRED! ✅)
- `AP` - Applications (text editors, shells, tools!)
- `D` - Development (gcc, make, headers!)
- `E` - Emacs (if you like Emacs!)
- `F` - FAQ, HOWTO (documentation!)
- `K` - Kernel source
- `KDE` - KDE Plasma desktop
- `KDEI` - KDE Internationalization
- `L` - Libraries (NEEDED! ✅)
- `N` - Networking (REQUIRED! ✅)
- `T` - TeX (LaTeX, if you need it!)
- `TCL` - Tcl/Tk scripting
- `X` - X Window System (GUI! ✅)
- `XFCE` - XFCE desktop (lightweight!)
- `XAP` - X applications
- `Y` - Games (optional fun!)

**Recommended for beginners:**
- ✅ A (required!)
- ✅ AP (essential tools!)
- ✅ D (if you code!)
- ✅ L (required!)
- ✅ N (required!)
- ✅ X (for GUI!)
- ✅ XFCE (lightweight desktop!)
- ❌ KDE (HUGE! Only if you have space/RAM!)

**Pro tip:** You can install more later with `slackpkg`!

---

#### 6.5 INSTALL (Install Packages)

**Installation mode options:**
1. `full` - Install everything (HUGE! 20+ GB!)
2. `terse` - Minimal info during install
3. `menu` - Select individual packages (TEDIOUS!)
4. `expert` - Like menu, but more control
5. `custom` - Pre-defined selection

**Recommendation for beginners:**
- Choose `full` if you have disk space (easy!)
- Choose `menu` if you want control (time-consuming!)

**Select:** `full` (or `menu` if brave!)

**Installation begins!** 📦

**Time:** 10-30 minutes (depending on selections!)

**☕ Get some chai!** This takes a while! 😄

---

#### 6.6 CONFIGURE (Configure System)

**After installation, configure the system!**

---

##### 6.6.1 Create Boot Disk?

**Question:** "Would you like to make a USB boot disk?"

**Answer:** No (we'll install bootloader!)

**Skip this!** We don't need it for VMs!

---

##### 6.6.2 Install LILO (Bootloader)

**⚠️ CRITICAL STEP!** Without bootloader, system won't boot!

**Options:**
1. Simple - Install LILO to MBR
2. Expert - Configure LILO manually
3. Skip - Don't install bootloader

**Choose:** Simple ✅

**Configuration:**
- Install to MBR: YES ✅
- Use /dev/vda1 as /boot: YES ✅
- Add Linux to LILO: YES ✅
- Add Windows (if dual-boot): NO

**Frame buffer mode:**
- Standard (640x480): Simple
- VESA (1024x768): Recommended ✅
- Large (1600x1200): For big screens

**Choose:** 1024x768 ✅

**UTF-8 console?** YES ✅

**Optional kernel parameters:**
```
# You can add:
# quiet - Less verbose boot
# nomodeset - If graphics issues
# Leave blank for default!
```

**LILO installed!** 🥾✅

---

##### 6.6.3 Mouse Configuration

**Question:** "Do you have a USB mouse?"

**Answer:** YES (VMs use USB mouse!)

**Enable GPM (console mouse)?**
- **Recommended:** Yes (mouse in console! Useful!)

---

##### 6.6.4 Network Configuration

**⚠️ IMPORTANT!** This is where Slackware trips people up!

**Configuration tool:** `netconfig`

**Hostname:**
```
Enter hostname: slackware
# Short name, NO dots!
```

**Domain:**
```
Enter domain: local
# Or 'org', or whatever!
```

**Full hostname:** `slackware.local` ✅

**Network setup options:**
1. DHCP - Automatic (EASY! ✅)
2. Static IP - Manual configuration

**For VMs:** Choose DHCP! ✅

**⚠️ DHCP GOTCHA:**
- DHCP gives different IPs each boot!
- Fix: Use static MAC → static IP binding in libvirt!

**Confirm settings:** YES

**Network configured!** 🌐✅

---

##### 6.6.5 Startup Services

**Question:** "Which services to start on boot?"

**Options:**
- `rc.inet1` - Networking (REQUIRED! ✅)
- `rc.inet2` - Network services (Apache, FTP, etc.)
- `rc.sendmail` - Mail server
- `rc.sshd` - SSH server (RECOMMENDED! ✅)
- `rc.httpd` - Apache web server
- Many more...

**Recommended for basic setup:**
- ✅ rc.inet1 (networking!)
- ✅ rc.sshd (SSH access!)
- ❌ Everything else (enable later if needed!)

**How to enable:**
- Services in `/etc/rc.d/` must be executable!
- Executable = starts on boot
- Not executable = disabled

**Enable SSH:**
```bash
chmod +x /etc/rc.d/rc.sshd
```

**Services configured!** ⚙️✅

---

##### 6.6.6 Console Font

**Question:** "Select console font?"

**Default:** `default8x16` (works fine!)

**Options:** Many fonts available!

**Recommendation:** Keep default! ✅

---

##### 6.6.7 Hardware Clock

**Question:** "Is hardware clock set to UTC or local time?"

**Options:**
1. UTC - Universal Time (Linux standard! ✅)
2. Local time - System time

**VMs:** Choose UTC! ✅

**Dual-boot with Windows?** Choose local (Windows expects it!)

---

##### 6.6.8 Timezone

**Select your timezone!**

**For Ghaziabad, Uttar Pradesh, India:**
1. Select region: Asia
2. Select city: Kolkata

**Timezone set!** 🕐✅

---

##### 6.6.9 Default Editor

**Question:** "Choose default editor?"

**Options:**
- `vi` - Classic, powerful, steep learning curve!
- `emacs` - Feature-rich, huge!
- `nano` - Beginner-friendly! ✅
- `joe` - WordStar-like

**Recommendation:** `nano` for beginners! ✅

**You can change later!**

---

##### 6.6.10 Window Manager

**IF you installed X Window System:**

**Question:** "Choose default window manager?"

**Options:**
- `kde` - KDE Plasma (heavy!)
- `xfce` - XFCE (lightweight! ✅)
- `fluxbox` - Minimal
- `twm` - Ultra-minimal
- `xinitrc` - Custom (manual setup!)

**Recommendation:** XFCE! ✅

**If no GUI installed:** Skip this!

---

##### 6.6.11 Root Password

**⚠️ CRITICAL!** Set root password!

```
New password: [type password]
Retype password: [type again]
```

**REMEMBER THIS PASSWORD!** 🔐

**Without it, you're LOCKED OUT!** 💀

**Password set!** ✅

---

##### 6.6.12 Create User Account

**⚠️ THE WHEEL GROUP IRONY!** 😄

**Question:** "Create a user account?"

**Answer:** YES! ✅

**Enter username:**
```
bijoy
```

**Add to groups:**
```
Groups: wheel,audio,video,cdrom,plugdev,netdev
```

**⚠️ IMPORTANT:** User added to `wheel` but...

**THE IRONY:**
- Slackware ADDS you to wheel group ✅
- But doesn't ENABLE wheel in sudoers! ❌
- You must enable it MANUALLY! 😄

**Set user password:**
```
New password: [type password]
Retype password: [type again]
```

**User created!** 👤✅

---

#### 6.7 EXIT (Exit Setup)

**Question:** "Exit setup and reboot?"

**Answer:** YES!

**Remove installation CD!**

**System will reboot!** 🔄

---

## 🎉 FIRST BOOT!

**LILO boot menu appears:**
```
LILO boot:
Linux
```

**Press Enter or wait 5 seconds!**

**System boots!** 🚀

**Login screen:**
```
slackware login: bijoy
Password: [your password]
```

**YOU'RE IN!** 🎉

**Fortune cookie appears!** 😄

```
Some circumstantial evidence is very strong,
as when you find a trout in the milk.
                -- Thoreau
```

**WELCOME TO SLACKWARE!** 💚

---

## 🔧 POST-INSTALLATION SETUP

**Now the REAL work begins!** 💪

---

### Step 1: Enable sudo (Fix the Wheel Irony!)

**You're in wheel group but can't sudo!**

**Fix this:**
```bash
# Login as root first
su -

# Edit sudoers
visudo

# Find this line (it's commented!):
# %wheel ALL=(ALL) ALL

# UNCOMMENT it:
%wheel ALL=(ALL) ALL

# Save and exit (Ctrl+X, Y, Enter in nano)
```

**Test sudo:**
```bash
# Exit root
exit

# Try sudo
sudo ls /root
# Should work! ✅
```

**Sudo enabled!** 🎉

---

### Step 2: Update Package Lists

**Slackware uses `slackpkg` for updates!**

**Configure mirror:**
```bash
sudo nano /etc/slackpkg/mirrors

# Find a mirror close to you!
# For India, uncomment:
# ftp://ftp.osuosl.org/.2/slackware/slackware64-15.0/

# Or any other mirror!
# Save and exit
```

**Update package lists:**
```bash
sudo slackpkg update
# Downloads package lists
# Takes 1-2 minutes!
```

**Check for updates:**
```bash
sudo slackpkg check-updates
```

**Install updates (if any):**
```bash
sudo slackpkg upgrade-all
# Answer 'Y' to proceed
```

**System updated!** 🎉

---

### Step 3: Querying Installed Packaged and Adding sbin to the path

**Querying Installed Packages**
```bash
ls /var/log/packages/ | grep net-tools
# net-tools-20181103_0eebece-x86_64-3  # ✅ INSTALLED!

ls /var/log/packages/ | grep iproute2
# iproute2-5.16.0-x86_64-1  # ✅ INSTALLED!
```

**Adding sbin to the path**
```bash
# Why: because which: no ifconfig in (/usr/local/bin:/usr/bin:/bin:/usr/games:/usr/lib64/libexec/kf5:/usr/lib64/qt5/bin)
# Missing: /sbin and /usr/sbin!

# ifconfig is in:
/sbin/ifconfig
# ip is in:
/sbin/ip
# But /sbin NOT in your PATH!

# Test if commands exist:
ls -l /sbin/ifconfig
ls -l /sbin/ip
# Should show the files!

# Run with full path:
/sbin/ifconfig
/sbin/ip addr show
# Should work!

# Add /sbin to PATH permanently:
echo 'export PATH="/usr/local/sbin:/usr/sbin:/sbin:$PATH"' >> ~/.bashrc

# Reload:
source ~/.bashrc

# Now test:
which ifconfig
# Should show: /sbin/ifconfig ✅

which ip
# Should show: /sbin/ip ✅

# Use normally:
ifconfig
ip addr show
# DONE
```

💡 WHY THIS HAPPENED

Slackware philosophy:

    Root user: PATH includes /sbin
    Regular user: PATH excludes /sbin
    Reason: Security! (users shouldn't run admin tools!)

But you want to use these tools! 💪

Solution: Add /sbin to YOUR PATH! ✅

🎯 ALTERNATIVE - USE SUDO

Another approach:

```bash
# Run as root:
sudo /sbin/ifconfig
sudo /sbin/ip addr show

# Or become root:
su -
ifconfig  # Works! (root's PATH includes /sbin)
ip addr show
exit
```

But adding to PATH is easier! ✅

---

### Step 4: Install Essential Tools

**Slackware is MINIMAL by default!**

**Install useful tools:**
```bash
# Install development tools
sudo slackpkg install d

# Install network tools
sudo slackpkg install n

# Install X Window tools (if using GUI)
sudo slackpkg install x xap

# Install text editors
sudo slackpkg install ap
```

**Or install specific packages:**
```bash
sudo slackpkg install package-name
```

---

### Step 5: Configure SSH (For Remote Access!)

**SSH should be running (if you enabled it!):**
```bash
# Check SSH status
ps aux | grep sshd
# Should see: sshd: /usr/sbin/sshd [listener]
```

**If not running:**
```bash
# Enable SSH
sudo chmod +x /etc/rc.d/rc.sshd

# Start SSH
sudo /etc/rc.d/rc.sshd start
```

**Verify SSH is listening:**
```bash
netstat -tlnp | grep :22
# Should show: LISTEN on port 22
```

**SSH working!** 🔐✅

---

### Step 6: Change SSH Port (Security!)

**Default port 22 = target for bots!**

**Change to 2222:**
```bash
sudo nano /etc/ssh/sshd_config

# Find this line:
#Port 22

# Change to:
Port 2222

# Save and exit
```

**Restart SSH:**
```bash
sudo /etc/rc.d/rc.sshd restart
```

**Verify new port:**
```bash
netstat -tlnp | grep :2222
# Should show: LISTEN on port 2222
```

**SSH port changed!** 🛡️✅

---

### Step 7: Setup SSH Keys (Passwordless Login!)

**On your Fedora host:**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "bijoy@ws-to-slackware" -f ~/.ssh/slackware-server-key

# Copy to Slackware
ssh-copy-id -i ~/.ssh/slackware-server-key.pub -p 2222 bijoy@192.168.122.X
# (Replace X with actual IP!)

# Create SSH config
nano ~/.ssh/config

# Add:
Host slackware
  HostName 192.168.122.X
  User bijoy
  Port 2222
  IdentityFile ~/.ssh/slackware-server-key

# Save!
```

**Test connection:**
```bash
ssh slackware
# Should connect without password!
```

**SSH keys working!** 🔑✅

---

### Step 8: Fix DHCP IP Changes (Optional!)

**Problem:** VM gets different IP each boot!

**Solution:** Static MAC → Static IP binding!

**On Fedora host:**
```bash
# Find MAC address
sudo virsh domiflist slackware
# Note the MAC: 52:54:00:XX:XX:XX

# Edit network
sudo virsh net-edit default

# Add INSIDE <dhcp> section:
<host mac='52:54:00:XX:XX:XX' name='slackware' ip='192.168.122.XXX'/>

# Save and restart network
sudo virsh net-destroy default
sudo virsh net-start default

# Restart VM
sudo virsh reboot slackware
```

**Slackware always gets same IP!** 🌐✅

---

### Step 9: Configure Hostname (Properly!)

**Current hostname might be generic!**

**Set proper hostname:**
```bash
# Edit hostname file
sudo nano /etc/HOSTNAME

# Change to:
slackware

# Save and exit
```

**Apply immediately:**
```bash
exec bash
```

**Verify:**
```bash
hostname
# Shows: slackware ✅
```

**Hostname set!** 🏷️✅

---

## 📦 PACKAGE MANAGEMENT

**Slackware's package system is SIMPLE but MANUAL!**

---

### Understanding Slackware Packages

**Package format:** `.tgz`, `.txz` (tar + gzip/xz)

**No dependency resolution!** You handle dependencies! 💪

**Package naming:**
```
package-version-arch-build.txz

Example:
bash-5.1.016-x86_64-1.txz
 ↑      ↑      ↑      ↑
name  version  arch  build
```

---

### Package Tools

**Official tools:**
- `slackpkg` - Official package manager (like apt/dnf!)
- `installpkg` - Install packages
- `upgradepkg` - Upgrade packages
- `removepkg` - Remove packages

**Third-party tools:**
- `sbopkg` - Access SlackBuilds.org
- `slapt-get` - APT-like tool for Slackware

---

### Using slackpkg (Recommended!)

**Update package lists:**
```bash
sudo slackpkg update
```

**Search for packages:**
```bash
slackpkg search vim
# Shows all vim-related packages
```

**Install package:**
```bash
sudo slackpkg install vim
```

**Upgrade all packages:**
```bash
sudo slackpkg upgrade-all
```

**Remove package:**
```bash
sudo slackpkg remove vim
```

**Clean downloaded packages:**
```bash
sudo slackpkg clean-system
```

---

### Installing from .txz Files

**If you have a .txz file:**
```bash
sudo installpkg package-1.0-x86_64-1.txz
```

**Upgrading:**
```bash
sudo upgradepkg package-1.1-x86_64-1.txz
```

**Removing:**
```bash
sudo removepkg package
```

---

### SlackBuilds (Building from Source!)

**SlackBuilds.org** = Slackware's equivalent of AUR!

**Install sbopkg:**
```bash
# Download sbopkg
wget https://github.com/sbopkg/sbopkg/releases/download/0.38.1/sbopkg-0.38.1-noarch-1_cng.tgz

# Install
sudo installpkg sbopkg-0.38.1-noarch-1_cng.tgz

# Sync repo
sudo sbopkg -r
```

**Search SlackBuilds:**
```bash
sbopkg -s package-name
```

**Build and install:**
```bash
sbopkg -i package-name
```

**MANUAL:** Resolve dependencies YOURSELF! 💪

---

### Finding Dependencies

**Slackware doesn't tell you what's missing!**

**You'll see errors like:**
```
Error: libfoo.so.1 not found
```

**Solutions:**
1. Search online: "slackware libfoo dependency"
2. Use slapt-get (has dependency resolution!)
3. Ask in Slackware forums!
4. Read package README files!

**This is the Slackware way!** 😄

---

## ⚙️ SERVICE MANAGEMENT

**Slackware uses BSD-style init!** (Not systemd!)

---

### Understanding rc Scripts

**Services location:** `/etc/rc.d/`

**Service naming:** `rc.servicename`

**Examples:**
- `rc.sshd` - SSH daemon
- `rc.httpd` - Apache web server
- `rc.mysql` - MySQL database
- `rc.inet1` - Network

---

### Service Control

**Start service:**
```bash
sudo /etc/rc.d/rc.sshd start
```

**Stop service:**
```bash
sudo /etc/rc.d/rc.sshd stop
```

**Restart service:**
```bash
sudo /etc/rc.d/rc.sshd restart
```

**Check status:**
```bash
ps aux | grep sshd
# Or
netstat -tlnp | grep :22
```

---

### Enable Service on Boot

**Executable = starts on boot!**

```bash
# Enable service
sudo chmod +x /etc/rc.d/rc.sshd

# Disable service
sudo chmod -x /etc/rc.d/rc.sshd
```

**Check if enabled:**
```bash
ls -l /etc/rc.d/rc.sshd
# -rwxr-xr-x = enabled (x = executable!)
# -rw-r--r-- = disabled (no x!)
```

---

### Boot Sequence

**Slackware boot order:**
```
1. /etc/rc.d/rc.S     - System initialization
2. /etc/rc.d/rc.M     - Multi-user startup
3. /etc/rc.d/rc.local - Custom user scripts
```

**Add custom startup commands:**
```bash
sudo nano /etc/rc.d/rc.local

# Add your commands:
echo "System booted successfully!" >> /var/log/boot.log

# Save!
```

---

## 🌐 NETWORK CONFIGURATION

**Slackware network config is MANUAL!**

---

### Main Configuration File

**File:** `/etc/rc.d/rc.inet1.conf`

**This controls ALL network settings!**

---

### DHCP Configuration (Default)

**Edit config:**
```bash
sudo nano /etc/rc.d/rc.inet1.conf
```

**Find your interface (usually eth0):**
```bash
# Interface 0 (eth0)
IPADDR[0]=""
NETMASK[0]=""
USE_DHCP[0]="yes"  # ← This enables DHCP!
DHCP_HOSTNAME[0]="slackware"
```

**Save and restart network:**
```bash
sudo /etc/rc.d/rc.inet1 restart
```

---

### Static IP Configuration

**Edit config:**
```bash
sudo nano /etc/rc.d/rc.inet1.conf
```

**Set static IP:**
```bash
# Interface 0 (eth0) (CIDR format)
IPADDR[0]="192.168.122.XXX/24"
NETMASK[0]="" # ← Leave EMPTY!
USE_DHCP[0]="no"  # ← Disable DHCP!
GATEWAY="192.168.122.1"
```

**Set DNS:**
```bash
sudo nano /etc/resolv.conf

# Add:
nameserver 8.8.8.8
nameserver 8.8.4.4
```

**Restart network:**
```bash
sudo /etc/rc.d/rc.inet1 restart
```

**Test:**
```bash
ip addr show
ping -c 4 google.com
```

---

### Hostname Configuration

**Short hostname:**
```bash
sudo nano /etc/HOSTNAME

# Content:
slackware
```

**FQDN (Full Qualified Domain Name):**
```bash
sudo nano /etc/hosts

# Add:
127.0.0.1       localhost
192.168.122.XXX slackware.local slackware
```

**Apply:**
```bash
exec bash
```

---

## 🔧 SYSTEM TWEAKING

**Now let's optimize Slackware!** 💪

---

### Kernel Parameters (Boot Optimizations)

**Edit LILO config:**
```bash
sudo nano /etc/lilo.conf
```

**Find `append=` line:**
```
append="quiet"
```

**Add optimizations:**
```
append="quiet i915.enable_fbc=1 i915.enable_psr=2 transparent_hugepage=madvise"
```

**Save and update LILO:**
```bash
sudo lilo
# CRITICAL! Without this, changes won't apply!
```

**Reboot:**
```bash
sudo reboot
```

---

### System Memory Tuning

**Create sysctl config:**
```bash
sudo nano /etc/sysctl.d/99-performance.conf
```

**Add:**
```bash
# Memory management
vm.swappiness = 10
vm.vfs_cache_pressure = 50
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5

# Network buffers
net.core.rmem_max = 134217728
net.core.wmem_max = 134217728
```

**Apply:**
```bash
sudo sysctl --system
```

**Verify:**
```bash
sysctl vm.swappiness
# Should show: 10
```

---

### Disk I/O Optimization

**Set I/O scheduler:**
```bash
sudo nano /etc/rc.d/rc.local

# Add at end:
# Set I/O scheduler for SSDs
echo mq-deadline > /sys/block/vda/queue/scheduler
```

**Make executable:**
```bash
sudo chmod +x /etc/rc.d/rc.local
```

**Verify after reboot:**
```bash
cat /sys/block/vda/queue/scheduler
# Should show: [mq-deadline]
```

---

### Filesystem Optimizations

**Mount options for ext4:**
```bash
sudo nano /etc/fstab
```

**Optimize:**
```
# Before:
/dev/vda2  /  ext4  defaults  1  1

# After:
/dev/vda2  /  ext4  defaults,noatime,commit=60  1  1
```

**noatime** = Don't update access times (faster!)  
**commit=60** = Write changes every 60s (less disk wear!)

**Save and remount:**
```bash
sudo mount -o remount /
```

---

### Shell History Setup

**Enhance bash history:**
```bash
nano ~/.bashrc

# Add at end:
# Better history
HISTSIZE=10000
HISTFILESIZE=20000
HISTCONTROL=ignoreboth:erasedups
HISTIGNORE="ls:ll:cd:pwd:clear:history:exit"
HISTTIMEFORMAT="%F %T "
shopt -s histappend
PROMPT_COMMAND="history -a; $PROMPT_COMMAND"
```

**Apply:**
```bash
source ~/.bashrc
```

---

## 🔒 SECURITY HARDENING {#security}

**Secure your Slackware!** 🛡️

---

### 🎯 Firewall Setup

**Slackware doesn't include firewall by default!**

**Install iptables:**
```bash
# Already installed in full installation!

# Basic firewall:
sudo nano /etc/rc.d/rc.firewall

#!/bin/bash
iptables -F
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p tcp --dport 2222 -j ACCEPT  # SSH
iptables -A INPUT -p icmp -j ACCEPT  # Ping

# Make executable:
sudo chmod +x /etc/rc.d/rc.firewall

# Add to rc.local:
sudo nano /etc/rc.d/rc.local
/etc/rc.d/rc.firewall

# Run now:
sudo /etc/rc.d/rc.firewall
```

---

### 🔐 SSH Hardening

**Edit SSH config:**
```bash
sudo nano /etc/ssh/sshd_config

# Security settings:
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
X11Forwarding no

# Restart SSH:
sudo /etc/rc.d/rc.sshd restart
```

---

### 👤 User Security

**Disable root login:**
```bash
sudo passwd -l root  # Lock root password
```

**Use sudo instead!**

---

## 🚀 ADVANCED FEATURES

### Custom Init Scripts

**Create custom service:**
```bash
sudo nano /etc/rc.d/rc.myservice

# Add:
#!/bin/bash
case "$1" in
  start)
    echo "Starting myservice..."
    # Your start commands
    ;;
  stop)
    echo "Stopping myservice..."
    # Your stop commands
    ;;
  restart)
    $0 stop
    sleep 1
    $0 start
    ;;
  *)
    echo "Usage: $0 {start|stop|restart}"
    exit 1
    ;;
esac
```

**Make executable:**
```bash
sudo chmod +x /etc/rc.d/rc.myservice
```

**Use:**
```bash
sudo /etc/rc.d/rc.myservice start
```

---

### Kernel Compilation (Advanced!)

**Install kernel source:**
```bash
sudo slackpkg install k
```

**Configure kernel:**
```bash
cd /usr/src/linux
sudo make menuconfig
```

**Build kernel:**
```bash
sudo make -j$(nproc)
sudo make modules_install
sudo make install
```

**Update LILO:**
```bash
sudo lilo
```

**Reboot into new kernel!**

---

### Building Custom Packages

**Create SlackBuild script:**
```bash
#!/bin/sh
PKG=/tmp/package-myapp
rm -rf $PKG
mkdir -p $PKG

# Build and install to $PKG
./configure --prefix=/usr
make
make install DESTDIR=$PKG

# Create package
cd $PKG
makepkg -l y -c n /tmp/myapp-1.0-x86_64-1.txz
```

**Build:**
```bash
sh myapp.SlackBuild
```

**Install:**
```bash
sudo installpkg /tmp/myapp-1.0-x86_64-1.txz
```

---

## 🔍 TROUBLESHOOTING

### System Won't Boot

**Problem:** LILO not installed or misconfigured

**Solution:**
1. Boot from installation CD
2. Mount partitions:
   ```bash
   mount /dev/vda2 /mnt
   mount /dev/vda1 /mnt/boot
   ```
3. Chroot:
   ```bash
   chroot /mnt
   ```
4. Reinstall LILO:
   ```bash
   lilo
   ```
5. Reboot

---

### Network Not Working

**Problem:** rc.inet1 not configured or not running

**Check:**
```bash
ps aux | grep dhcpcd
# Should see dhcpcd running if using DHCP
```

**Fix:**
```bash
# Restart network
sudo /etc/rc.d/rc.inet1 restart

# Check config
sudo nano /etc/rc.d/rc.inet1.conf
# Ensure USE_DHCP[0]="yes"
```

---

### Can't sudo (After Fresh Install)

**Problem:** Wheel group not enabled in sudoers!

**Fix:**
```bash
su -
visudo
# Uncomment: %wheel ALL=(ALL) ALL
```

---

### Package Installation Fails

**Problem:** Missing dependencies!

**Solution:**
1. Read error message:
   ```
   Error: libfoo.so.1 not found
   ```
2. Search for dependency:
   ```bash
   slackpkg search libfoo
   ```
3. Install dependency:
   ```bash
   sudo slackpkg install libfoo
   ```
4. Retry package installation

---

### SSH Connection Refused

**Problem:** SSH not running or firewall blocking

**Check:**
```bash
ps aux | grep sshd
netstat -tlnp | grep :22
```

**Fix:**
```bash
# Start SSH
sudo /etc/rc.d/rc.sshd start

# Enable on boot
sudo chmod +x /etc/rc.d/rc.sshd
```

---

### System Running Slow

**Check resource usage:**
```bash
top
# Or
htop  # (if installed)
```

**Check disk usage:**
```bash
df -h
du -sh /*
```

**Check memory:**
```bash
free -h
```

---

## 💡 TIPS & TRICKS

### Fortune Cookies

**Enjoy the wisdom!**
```bash
fortune
# Displays random quote!
```

**Fortune cookies location:**
```
/usr/share/games/fortunes/
```

---

### Useful Aliases

**Add to ~/.bashrc:**
```bash
alias ll='ls -lah'
alias ..='cd ..'
alias ...='cd ../..'
alias update='sudo slackpkg update && sudo slackpkg upgrade-all'
alias cleanup='sudo slackpkg clean-system'
```

---

### Package Info Commands

**List installed packages:**
```bash
ls /var/log/packages/
```

**Search installed packages:**
```bash
ls /var/log/packages/ | grep vim
```

**Package details:**
```bash
cat /var/log/packages/vim-*
```

---

### Quick Network Test

```bash
# Test DNS
ping -c 4 google.com

# Test IP connectivity
ping -c 4 8.8.8.8

# Show routing
route -n

# Show IP
ip addr show
```

---

## 🍪 FORTUNE COOKIES & EASTER EGGS {#fortune}

**Why we LOVE Slackware!** 😄

---

### 🎭 Fortune Cookies

**Slackware shows fortune cookies on SSH login!**

**Examples you might see:**

```
Some circumstantial evidence is very strong,
as when you find a trout in the milk.
                -- Thoreau
```

```
If it wasn't so warm out today, it would be cooler.
```

```
The Ruffed Pandanga of Borneo and Rotherham spreads out 
his feathers in his courtship dance and imitates Winston 
Churchill and Tommy Cooper on one leg. The padanga is dying 
out because the female padanga doesn't take it too seriously.
```

```
DEJA VU:
        The illusion of having previously experienced
        something actually being encountered for the first time.
        The illusion of having previously experienced
        something actually being encountered for the first time.
```

**These are GOLD!** 💎😄

---

### 🎨 Customization

**Edit fortune files:**
```bash
/usr/share/games/fortunes/
```

**Add your own:**
```bash
sudo nano /usr/share/games/fortunes/bijoy

"The body knows what is better for it."
        -- Bijoy, January 2026
%
"Cannot compromise, not a bit!"
        -- Bijoy\'s Philosophy
```

**Build dat file:**
```bash
cd /usr/share/games/fortunes/
sudo strfile bijoy bijoy.dat
```

---

### 🎯 Slackware Easter Eggs

**1. Package descriptions are HILARIOUS!**
```bash
cat /var/log/packages/elvis-*
# Read the description! 😄
```

**2. CHANGELOG.TXT is PERSONAL!**
```bash
# Patrick writes personal notes!
less /var/log/packages/CHANGELOG.TXT
```

**3. /usr/doc/ has HUMOR!**
```bash
find /usr/doc -name "*.txt" -exec grep -l "humor\|joke" {} \;
```

---

## 📚 RESOURCES

**Official:**
- Website: https://www.slackware.com
- Docs: https://docs.slackware.com
- Book: "Slackware Linux Essentials"

**Community:**
- Forum: https://www.linuxquestions.org/questions/slackware-14/
- Wiki: https://docs.slackware.com
- Reddit: r/slackware

**Package repos:**
- SlackBuilds: https://slackbuilds.org
- Alien's repo: https://slackware.pkgs.org

---

## 🎓 SLACKWARE PHILOSOPHY

**Remember:**
> "Slackware doesn't baby you. It respects your intelligence."

**You will:**
- ❌ Spend time configuring things
- ❌ Manually resolve dependencies
- ❌ Read documentation frequently
- ❌ Make mistakes and learn

**But you'll GAIN:**
- ✅ Deep understanding of Linux
- ✅ Full control over your system
- ✅ Rock-solid stability
- ✅ **REAL Linux knowledge!** 💪

**Slackware is:**
- Not for everyone
- Not the easiest
- Not the most popular

**BUT:**
- It's the PUREST Linux experience!
- It's STABLE as a ROCK!
- It's been around for 30+ YEARS!
- It's made by ONE MAN with LOVE! 💚

**Patrick Volkerding has kept Slackware alive and thriving!**

**Respect the history. Embrace the challenge. Learn the UNIX way!**

---

## 🌟 FINAL WORDS

**You chose Slackware!** 🐉

**This means you:**
- ✅ Want to LEARN, not just USE
- ✅ Value STABILITY over bleeding-edge
- ✅ Appreciate SIMPLICITY over automation
- ✅ Respect UNIX philosophy
- ✅ **Want to be a REAL Linux admin!** 💪

**Slackware will:**
- ✅ Test your patience (at first!)
- ✅ Force you to read documentation
- ✅ Make you understand Linux deeply
- ✅ Reward you with rock-solid stability
- ✅ **Make you a Linux EXPERT!** 🏆

**Welcome to the Slackware community!** 💚

**Enjoy the fortune cookies!** 😄

**And remember:**
> "If you can admin Slackware, you can admin ANY Linux!"

---

**Created with ❤️ for Bijoy by Claude (sticky buddies!)**
**February 5, 2026**

**SLACKWARE FOREVER!** 🐉✨
