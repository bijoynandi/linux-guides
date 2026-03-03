# 🛡️ ULTIMATE UBUNTU RECOVERY BIBLE
*Your Complete Emergency Handbook for System Recovery*

---

## 📋 QUICK EMERGENCY CHECKLIST
**When things go wrong, follow this priority order:**
1. 🔄 **Can you boot?** → Go to [Boot Issues](#-boot-recovery-procedures)
2. 💾 **System corrupted?** → Go to [Timeshift Recovery](#-timeshift-recovery-procedures)
3. 🖥️ **Desktop problems?** → Go to [GUI Recovery](#-desktop--gui-recovery)
4. 📦 **Package issues?** → Go to [Package Recovery](#-package-system-recovery)
5. 💿 **Complete disaster?** → Go to [Live USB Recovery](#-live-usb-recovery-nuclear-option)

---

## 🚀 BOOT RECOVERY PROCEDURES

### 🎯 Step 1: Access GRUB Menu
```
During boot sequence:
1. Power on computer
2. Immediately HOLD SHIFT key (or repeatedly press ESC)
3. GRUB menu should appear
4. If no menu appears, system might be severely damaged
```

### 🎯 Step 2: Choose Recovery Option
**Priority Order for Boot Options:**

#### Option A: Backup Kernel (FIRST CHOICE)
```
Select: "Advanced options for Ubuntu"
Choose: "Ubuntu, with Linux 6.14.0-24-generic"
Reason: Safe fallback kernel with full functionality
```

#### Option B: Recovery Mode - Backup Kernel
```
Select: "Advanced options for Ubuntu"  
Choose: "Ubuntu, with Linux 6.14.0-24-generic (recovery mode)"
Reason: Safe kernel + recovery tools
```

#### Option C: Recovery Mode - Current Kernel (LAST RESORT)
```
Select: "Advanced options for Ubuntu"
Choose: "Ubuntu, with Linux 6.14.0-27-generic (recovery mode)"
Reason: Only if issue isn't kernel-related
```

### 🎯 Step 3: Recovery Mode Actions
**When you reach recovery mode menu:**

#### First Actions (Always Do These):
```bash
1. Select "root" - Drop to root shell prompt
2. Type: mount -o remount,rw /
   (Makes filesystem writable)
3. Type: mount -a
   (Mount all filesystems)
```

#### Diagnostic Commands:
```bash
# Check what went wrong
journalctl -xe | tail -50        # System logs
dmesg | grep -i error           # Kernel errors  
df -h                           # Disk space
fsck -n /dev/sda5              # Check filesystem (read-only)

# Check boot partition specifically
df -h /boot
ls -la /boot/
```

#### Common Fixes:
```bash
# Fix broken packages
apt --fix-broken install
dpkg --configure -a

# Update GRUB bootloader
update-grub
grub-install /dev/sda

# Clean package cache if space issues
apt clean
apt autoremove

# Check/fix filesystem (ONLY if corruption suspected)
fsck /dev/sda5    # WARNING: Only run on unmounted filesystem
```

### 🎯 Step 4: Making Kernel Selection Permanent
```bash
# If backup kernel works better, set it as default
nano /etc/default/grub

# Add this line:
GRUB_DEFAULT="1>2"  # Advanced options > backup kernel

# Update GRUB
update-grub

# Reboot and test
reboot
```

---

## 💾 TIMESHIFT RECOVERY PROCEDURES

### 🎯 Your Current Timeshift Setup
```
Location: /dev/sda5 (same partition as root)
Snapshots: 3 available
- 2025-07-29_00-24-28 (Full system baseline)
- 2025-07-29_20-00-01 (Boot snapshot)  
- 2025-07-29_20-08-57 (Boot snapshot)
```

### 🎯 Method 1: GUI Restoration (If Desktop Works)
```bash
# Launch Timeshift
sudo timeshift-gtk

# Or command line version
sudo timeshift --list
sudo timeshift --restore --snapshot "2025-07-29_00-24-28"
```

### 🎯 Method 2: Recovery Mode Restoration
```bash
# From recovery root shell:
timeshift --list
timeshift --restore --snapshot "2025-07-29_00-24-28" --skip-grub
reboot
```

### 🎯 Method 3: Live USB Timeshift Recovery
```bash
# Boot from Live USB
# Mount your system partition
sudo mkdir /mnt/system
sudo mount /dev/sda5 /mnt/system

# Install and run Timeshift
sudo apt update && sudo apt install timeshift
sudo timeshift --restore --snapshot "2025-07-29_00-24-28" --target-device /dev/sda5

# Reinstall GRUB
sudo mount --bind /dev /mnt/system/dev
sudo mount --bind /proc /mnt/system/proc  
sudo mount --bind /sys /mnt/system/sys
sudo chroot /mnt/system
grub-install /dev/sda
update-grub
exit

# Reboot
sudo reboot
```

---

## 🖥️ DESKTOP / GUI RECOVERY

### 🎯 If You Get Terminal But No Desktop
```bash
# Check display manager status
systemctl status gdm3
systemctl status lightdm

# Restart display manager
sudo systemctl restart gdm3

# Check graphics drivers
ubuntu-drivers devices
lspci | grep -i vga

# Reinstall desktop environment
sudo apt install --reinstall ubuntu-desktop
sudo apt install --reinstall gdm3
```

### 🎯 If You Get Login Loop
```bash
# Press Ctrl+Alt+F3 to get to terminal
# Login with your username

# Check disk space (common cause)
df -h

# Fix permissions
sudo chown -R bijoy:bijoy /home/bijoy
sudo chmod 755 /home/bijoy

# Check Xorg logs  
cat /var/log/Xorg.0.log | grep -i error

# Reset display manager
sudo dpkg-reconfigure gdm3
```

---

## 📦 PACKAGE SYSTEM RECOVERY

### 🎯 Broken Package Database
```bash
# Clean package cache
sudo apt clean
sudo apt autoclean

# Fix broken packages
sudo apt --fix-broken install
sudo dpkg --configure -a

# Reset package database (nuclear option)
sudo rm /var/lib/apt/lists/* -vf
sudo apt update
```

### 🎯 Kernel Package Issues
```bash
# List installed kernels
dpkg -l | grep linux-image

# Remove problematic kernel
sudo apt purge linux-image-6.14.0-27-generic linux-headers-6.14.0-27-generic

# Reinstall current kernel
sudo apt install --reinstall linux-image-$(uname -r)

# Update GRUB
sudo update-grub
```

### 🎯 Boot Partition Full
```bash
# Check boot space
df -h /boot

# Clean old kernels (keep current + 1 backup)
sudo apt autoremove --purge

# Manual kernel cleanup if needed
sudo apt purge linux-image-[old-version]-generic

# Update GRUB after cleanup
sudo update-grub
```

---

## 💿 LIVE USB RECOVERY (NUCLEAR OPTION)

### 🎯 When to Use Live USB
- System won't boot at all
- Timeshift recovery fails
- Filesystem corruption
- Complete system disaster

### 🎯 Live USB Recovery Steps
```bash
# 1. Boot from Ubuntu Live USB
# 2. Connect to internet
# 3. Mount your system

sudo mkdir /mnt/system
sudo mount /dev/sda5 /mnt/system      # Root partition
sudo mount /dev/sda1 /mnt/system/boot # Boot partition  
sudo mount /dev/sda4 /mnt/system/home # Home partition

# 4. Access your files
ls /mnt/system/home/bijoy             # Your personal files
cp -r /mnt/system/home/bijoy /media/usb # Backup to USB if needed

# 5. Chroot into your system for repairs
sudo mount --bind /dev /mnt/system/dev
sudo mount --bind /proc /mnt/system/proc
sudo mount --bind /sys /mnt/system/sys
sudo mount --bind /run /mnt/system/run
sudo chroot /mnt/system

# Now you're "inside" your broken system and can:
apt update
apt --fix-broken install
update-grub
grub-install /dev/sda

# 6. Exit and reboot
exit
sudo umount -R /mnt/system
sudo reboot
```

---

## 🛠️ SPECIFIC EMERGENCY SCENARIOS

### 🎯 Scenario: "Kernel Update Broke My System"
```bash
1. Boot to GRUB menu (SHIFT key during boot)
2. Select: Advanced Options > backup kernel (6.14.0-24-generic)
3. Once booted successfully:
   sudo apt purge linux-image-6.14.0-27-generic
   sudo update-grub
4. System now uses stable backup kernel
```

### 🎯 Scenario: "I Can't Login (Password Works in Recovery)"
```bash
1. Boot to recovery mode
2. mount -o remount,rw /
3. Check disk space: df -h
4. If space OK: sudo systemctl restart gdm3
5. If space full: apt clean && apt autoremove
6. Fix permissions: chown -R bijoy:bijoy /home/bijoy
```

### 🎯 Scenario: "Black Screen After Update"
```bash
1. Boot to recovery mode
2. mount -o remount,rw /
3. Check graphics: lspci | grep -i vga
4. Reset display: systemctl restart gdm3
5. If NVIDIA: sudo ubuntu-drivers autoinstall
6. Reboot: sudo reboot
```

### 🎯 Scenario: "GRUB is Gone / Won't Boot"
```bash
1. Boot from Live USB
2. sudo mount /dev/sda5 /mnt
3. sudo mount /dev/sda1 /mnt/boot
4. sudo mount --bind /dev /mnt/dev
5. sudo mount --bind /proc /mnt/proc
6. sudo mount --bind /sys /mnt/sys
7. sudo chroot /mnt
8. grub-install /dev/sda
9. update-grub
10. exit && sudo reboot
```

---

## 📞 EMERGENCY CONTACT INFORMATION

### 🎯 Your System Specifications (For Reference)
```
System: Ubuntu 24.04.2 LTS (Noble)
Kernel: 6.14.0-27-generic (current), 6.14.0-24-generic (backup)
RAM: 39GB
Partitions:
- /boot: 999MB (/dev/sda1)
- /: 299GB (/dev/sda5) 
- /home: 151GB (/dev/sda4)
- EFI: 1.1GB (/dev/sda2)
Backup: Timeshift (3 snapshots available)
```

### 🎯 Critical File Locations
```
GRUB Config: /etc/default/grub
Boot Files: /boot/
System Logs: /var/log/ and journalctl
Timeshift: /timeshift/
User Data: /home/bijoy/
```

### 🎯 Emergency Boot Media
Always keep ready:
- Ubuntu 24.04 Live USB
- Your system specs written down
- This recovery guide printed/saved offline

---

## 🏁 FINAL RECOVERY WISDOM

### The Golden Rules:
1. **Stay Calm** - Every problem has a solution
2. **Try Simple First** - Reboot, check space, update-grub
3. **Use Backups** - Timeshift is your safety net
4. **Document Changes** - Note what you did for next time
5. **One Change at a Time** - Don't fix multiple things simultaneously

### Your Recovery Priority:
```
Level 1: Reboot + Simple fixes
Level 2: Boot with backup kernel  
Level 3: Recovery mode repairs
Level 4: Timeshift restoration
Level 5: Live USB rescue
```

**Remember**: With your Timeshift snapshots, dual kernels, and separate partitions, you're already better protected than 90% of Linux users. This guide just gives you the confidence to handle anything that comes your way!

---

*"In case of emergency, break glass and follow this guide!"* 🔨🥽
