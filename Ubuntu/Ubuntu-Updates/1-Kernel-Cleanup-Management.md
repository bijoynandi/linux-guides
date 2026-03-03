# 🧹 SAFE KERNEL CLEANUP & MANAGEMENT

## Step 1: Check What Kernels You Have
```bash
# List all installed kernels
dpkg -l | grep linux-image

# See which kernel you're currently running
uname -r

# Check boot partition space
df -h /boot
```

## Step 2: See What's Taking Space in /boot
```bash
# Detailed view of boot partition contents
ls -lah /boot/

# Size of each kernel
du -sh /boot/*
```

## Step 3: Clean Old Kernels (SAFELY)
```bash
# Option 1: Let Ubuntu handle it automatically
sudo apt autoremove

# Option 2: Manual cleanup (if autoremove doesn't work)
# First, list kernels to remove (keep current + 1 backup)
sudo apt list --installed | grep linux-image

# Remove specific old kernel (replace X.X.X with actual version)
sudo apt purge linux-image-X.X.X-generic linux-headers-X.X.X-generic

# Clean up orphaned packages
sudo apt autoremove
sudo apt autoclean
```

## Step 4: Configure Automatic Kernel Cleanup
```bash
# Edit the configuration file
sudo nano /etc/apt/apt.conf.d/01autoremove-kernels

# Add this line to keep only 2 kernels (current + 1 backup):
# Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
```

## Step 5: Update GRUB (Important!)
```bash
# After removing kernels, update GRUB
sudo update-grub

# Verify GRUB shows correct kernels
grep menuentry /boot/grub/grub.cfg
```

## Emergency Recovery Commands
If something goes wrong and you can't boot:

### From Recovery Mode:
```bash
# Check if root filesystem is mounted read-only
mount | grep "on / "

# Remount as read-write if needed
mount -o remount,rw /

# Reinstall current kernel
apt install --reinstall linux-image-$(uname -r)

# Update GRUB
update-grub
```

### Prevention Settings
```bash
# Create backup script for /boot before kernel updates
sudo nano /usr/local/bin/backup-boot.sh

#!/bin/bash
# Backup boot partition before kernel updates
cp -r /boot /boot.backup.$(date +%Y%m%d)
echo "Boot partition backed up to /boot.backup.$(date +%Y%m%d)"

# Make it executable
sudo chmod +x /usr/local/bin/backup-boot.sh
```

## Pro Tips for Future Kernel Updates

### 1. Check Boot Space Before Updates
```bash
# Add this to your update routine
df -h /boot && echo "Boot partition space OK" || echo "⚠️  Boot partition getting full!"
```

### 2. Kernel Update Best Practices
- Always check `/boot` space first
- Keep at least 100MB free in `/boot`
- Never remove the currently running kernel
- Always keep at least 1 backup kernel
- Test new kernels in VM first (like you did!)
- Reboot soon after kernel updates to test

### 3. Set Up Monitoring
```bash
# Add to crontab to monitor boot space
crontab -e

# Add this line (check daily at 9 AM):
0 9 * * * df -h /boot | awk 'NR==2 {if($5+0 > 80) print "⚠️  Boot partition " $5 " full!"}'
```

## Understanding Your System Better

### Current Situation Analysis:
- **Boot partition**: 999MB total, 204MB used (20%) - Still healthy!
- **Root partition**: 299GB total, 121GB used (43%) - Plenty of space
- **Home partition**: 151GB total, 17GB used (12%) - Lots of room
- **Memory**: 39GB total, 2.1GB used - Excellent!

### Why Multiple Partitions?
Your setup is actually quite good:
- `/boot` (999MB) - Kernels and boot files
- `/` (299GB) - System files and programs
- `/home` (151GB) - Your personal files
- `/boot/efi` (1.1GB) - UEFI boot loader

This separation means if one partition has issues, others stay safe.
