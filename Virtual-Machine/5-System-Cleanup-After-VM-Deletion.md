# 🧹 Complete VM Cleanup Guide - Windows & Ubuntu

*The ultimate guide to completely remove your VMs and prepare for fresh installations!* 🎯

---

## 🚨 Before You Start - CRITICAL WARNING!

**⚠️ THIS WILL PERMANENTLY DELETE YOUR VM AND ALL DATA INSIDE IT!**

Make sure you:
- ✅ Backed up any important files from VM
- ✅ Exported any settings you want to keep
- ✅ Are absolutely sure you want to delete everything
- ✅ Have your ISO ready for reinstallation (if needed)

---

## The Ultimate Lazy-Efficient Method! 😎
For future cleanups, your optimized process:

```bash
# The "Bijoy Method" - Terminal + GUI hybrid!
VM_NAME="windows-vm"

# Step 1: Terminal cleanup of manual configs (30 seconds)
sudo shutdown $VM_NAME                     # Clean shutdown
sudo sed -i '/WindowsC/d' /etc/fstab       # Remove YOUR fstab entry
sudo rm -f /etc/cifs-credentials           # Remove YOUR credentials
sudo rm -rf /mnt/windows-vm                 # Remove YOUR mount point
sudo virsh net-destroy default && sudo virsh net-start default  # Reset network


# Step 2: GUI cleanup (10 seconds)
# Right-click VM → Delete → ✅ Check Delete associated storage files  → Delete → Done!

echo "🎉 VM nuked from orbit! Both ways!"
```
---

## 🔍 Step 1: Locate Your VM (Manual Detailed Way to Clean Up the VM)

First, let's find exactly what we're dealing with:

```bash
# List all VMs and their status
virsh list --all
```

```bash
# Find your VM disk files
sudo find /var/lib/libvirt/images/ -name "*.qcow2" -o -name "*.img"
```

```bash
# Check the size of your VM disk (multiple methods for accuracy)
sudo ls -lh /var/lib/libvirt/images/windows-vm.qcow2

# Better method - shows actual disk usage vs shell issues
sudo sh -c 'du -sh /var/lib/libvirt/images/*'

# Best method - shows virtual vs actual disk usage
sudo qemu-img info /var/lib/libvirt/images/windows-vm.qcow2
```

**What this shows:** Your VM name, disk location, virtual size vs actual disk usage, and QCOW2 efficiency!

📝 **Write down:**
- VM Name: `windows-vm` (or whatever yours is called)
- Disk location: `/var/lib/libvirt/images/windows-vm.qcow2`
- Virtual size: (e.g., 40 GiB - what Windows sees)
- Actual disk size: (e.g., 17.6 GiB - what you'll actually free up!)
- File format: (QCOW2 with thin provisioning)

---

## 🛑 Step 2: Stop the VM

If your VM is running, we need to shut it down properly:

### Option A: Graceful Shutdown (Recommended)
```bash
# Send shutdown command to VM
virsh shutdown windows-vm
```

**What this does:** Tells Windows to shut down normally (like clicking Start → Shutdown)

### Option B: Force Stop (if VM is frozen)
```bash
# Force stop the VM immediately
virsh destroy windows-vm
```

**What this does:** Immediately cuts power to VM (like pulling the plug)

### Check VM performance and disk activity (if running):
```bash
# Check disk I/O statistics (only works if VM is running)
virsh domblkstat windows-vm
```

**What this shows:** Read/write operations, bytes transferred, and disk performance metrics.

### Verify it's stopped:
```bash
# Check VM status - should show "shut off"
virsh list --all

# Verify disk I/O has stopped (this should now give an error if VM is stopped)
virsh domblkstat windows-vm  # Should show "domain is not running" when stopped
```

---

## 🗑️ Step 3: Delete VM Configuration

Now let's remove the VM configuration from the system:

```bash
# Delete VM configuration only (keeps disk files)
virsh undefine windows-vm
```

**What this does:** Removes the VM from libvirt's database but keeps the disk files safe.

### Verify deletion:
```bash
# VM should no longer appear in the list
virsh list --all
```

---

## 💾 Step 4: Delete VM Storage (The Big Files!)

This is where we free up the actual disk space:

### Option A: Manual Deletion (Recommended for beginners)

### Check storage before deletion:
```bash
# Check what we're about to delete - multiple verification methods
sudo ls -lh /var/lib/libvirt/images/windows-vm.qcow2
sudo sh -c 'du -sh /var/lib/libvirt/images/windows-vm.qcow2'
sudo qemu-img info /var/lib/libvirt/images/windows-vm.qcow2

# Delete the VM disk file
sudo rm /var/lib/libvirt/images/windows-vm.qcow2
```

**Pro tip:** The `qemu-img info` shows you exactly how much **real** disk space you'll recover!

### Option B: Advanced - Delete VM with storage in one command
```bash
# Stop and delete VM + storage in one go (use with caution!)
virsh shutdown windows-vm && sleep 10 && virsh undefine windows-vm --remove-all-storage
```

### Check storage is freed:
```bash
# Should show no Windows-related files
sudo find /var/lib/libvirt/images/ -name "*windows*"

# Verify the exact space freed up
sudo sh -c 'du -sh /var/lib/libvirt/images/*' 2>/dev/null || echo "All VM files cleaned!"

# Check overall disk space improvement
df -h /var/lib/libvirt/images/
```

---

## 🧹 Step 5: Clean Up Ubuntu Mount Configuration (Windows VMs Only)

**Skip this section if you're deleting Ubuntu/Linux VMs - they don't need this!**

Remove all Windows VM mounting setup from Ubuntu:

### Remove fstab entry:
```bash
# Backup your fstab first (safety first!)
sudo cp /etc/fstab /etc/fstab.backup.$(date +%Y%m%d)

# Edit fstab to remove Windows mount line
sudo nano /etc/fstab
```

**In nano:** Look for and DELETE lines containing `WindowsC` or your Windows share name:
```bash
# DELETE THIS LINE:
//192.168.122.XXX/WindowsC /mnt/windows-vm cifs credentials=/etc/cifs-credentials,uid=bijoy,gid=bijoy,iocharset=utf8,file_mode=0777,dir_mode=0777,_netdev,x-systemd.automount,x-systemd.device-timeout=30 0 0
```

**Save and exit:** `Ctrl+X` → `Y` → `Enter`

### Remove credentials file:
```bash
# Delete the Windows credentials file
sudo rm -f /etc/cifs-credentials
```

### Remove mount point:
```bash
# Remove the mount directory
sudo rmdir /mnt/windows-vm

# If it says "not empty", check what's inside first:
ls -la /mnt/windows-vm
# Then force remove if safe:
sudo rm -rf /mnt/windows-vm
```

### Update system configuration:
```bash
# Reload systemd to forget about the mount
sudo systemctl daemon-reload
```

---

## 🏗️ Step 6: Clean Up Network Configuration (Optional)

If you want to clean up network settings too:

```bash
# Remove any DHCP leases for the deleted VM
sudo virsh net-dhcp-leases default

# If you see your old VM IP, restart the network:
sudo virsh net-destroy default
sudo virsh net-start default
```

**What this does:** Clears old IP assignments so your next VM can get a clean network setup.

---

## 🐧 BONUS: Simple Ubuntu VM Cleanup

**For Ubuntu/Linux VMs, the process is much simpler - no Windows networking complexity!**

### Quick Ubuntu VM Removal:

```bash
# 1. Find your Ubuntu VM
virsh list --all
sudo sh -c 'du -sh /var/lib/libvirt/images/*'
sudo qemu-img info /var/lib/libvirt/images/ubuntu-vm.qcow2

# 2. Stop the VM
virsh shutdown ubuntu-vm  # Replace with your Ubuntu VM name

# 3. Delete VM AND storage in one command
virsh undefine ubuntu-vm --remove-all-storage

# 4. Verify cleanup
virsh list --all  # Should show VM is gone
df -h /var/lib/libvirt/images/  # Should show freed space
```

**Why Ubuntu cleanup is simpler:**
- ❌ No fstab entries to clean
- ❌ No Windows credential files
- ❌ No complex network shares
- ❌ No Windows-specific mount points

**Just stop → delete → done!** 🎯

---

## ✅ Step 7: Verify Complete Cleanup

Let's make sure everything is gone:

### Check VM is completely removed:
```bash
# Should show no VMs or only other VMs (not the deleted one)
virsh list --all
```

### Check storage is freed:
```bash
# Should show no VM-related files
sudo find /var/lib/libvirt/images/ -name "*windows*"
sudo find /var/lib/libvirt/images/ -name "*ubuntu*"

# Check available disk space (should be more now!)
df -h /var/lib/libvirt/images/

# See exactly what's left
sudo sh -c 'du -sh /var/lib/libvirt/images/*' 2>/dev/null || echo "Directory is clean!"
```

### Check Ubuntu mount cleanup (Windows VMs only):
```bash
# Should return nothing
cat /etc/fstab | grep -i windows

# Should not exist
ls /etc/cifs-credentials

# Should not exist
ls /mnt/windows-vm
```

### Check freed space:
```bash
# See how much space you freed up
df -h /
```

---

## 🎊 Step 8: Final System Cleanup

Optional but recommended final touches:

```bash
# Clean package cache
sudo apt autoremove
sudo apt autoclean

# Update locate database
sudo updatedb

# Clear any leftover temp files
sudo rm -rf /tmp/*libvirt* 2>/dev/null
```

---

## 🚀 Automated Cleanup Scripts

### Windows VM Cleanup Script:

```bash
# Create the cleanup script
nano ~/windows-vm-cleanup.sh
```

**Add this content:**
```bash
#!/bin/bash

VM_NAME="windows-vm"
echo "🧹 Starting complete cleanup of $VM_NAME..."

# Show current disk usage
echo "📊 Current VM disk usage:"
sudo qemu-img info /var/lib/libvirt/images/$VM_NAME.qcow2

# Backup fstab
sudo cp /etc/fstab /etc/fstab.backup.$(date +%Y%m%d)
echo "📋 Backed up fstab"

# Stop VM if running
echo "🛑 Stopping VM..."
virsh shutdown $VM_NAME 2>/dev/null
sleep 10

# Delete VM and storage
echo "🗑️ Deleting VM configuration and storage..."
virsh undefine $VM_NAME --remove-all-storage 2>/dev/null

# Clean Ubuntu mount setup
echo "🧽 Cleaning mount configuration..."
sudo sed -i '/WindowsC/d' /etc/fstab
sudo sed -i '/windows-vm/d' /etc/fstab
sudo rm -f /etc/cifs-credentials
sudo rm -rf /mnt/windows-vm

# Reload system config
sudo systemctl daemon-reload

# Network cleanup
echo "🌐 Cleaning network configuration..."
sudo virsh net-destroy default 2>/dev/null
sudo virsh net-start default 2>/dev/null

echo "✅ Cleanup complete!"
echo "💾 Freed storage space:"
df -h /var/lib/libvirt/images/
echo "🎯 System ready for fresh VM installation!"
```

### Ubuntu VM Cleanup Script:

```bash
# Create the Ubuntu cleanup script
nano ~/ubuntu-vm-cleanup.sh
```

**Add this content:**
```bash
#!/bin/bash

VM_NAME="ubuntu-vm"
echo "🐧 Starting Ubuntu VM cleanup of $VM_NAME..."

# Show current disk usage
echo "📊 Current VM disk usage:"
sudo qemu-img info /var/lib/libvirt/images/$VM_NAME.qcow2

# Stop and delete VM
echo "🛑 Stopping and deleting VM..."
virsh shutdown $VM_NAME && sleep 10 && virsh undefine $VM_NAME --remove-all-storage

echo "✅ Ubuntu VM cleanup complete!"
echo "💾 Freed storage space:"
df -h /var/lib/libvirt/images/
echo "🚀 Much simpler than Windows, right?"
```

**Make scripts executable and run:**
```bash
chmod +x ~/windows-vm-cleanup.sh
chmod +x ~/ubuntu-vm-cleanup.sh

# Run the appropriate one:
./windows-vm-cleanup.sh
# OR
./ubuntu-vm-cleanup.sh
```

---

## 🎯 Post-Cleanup Checklist

After running the cleanup, verify everything is clean:

### For Any VM Type:
- ✅ **VM Gone:** `virsh list --all` shows no deleted VM
- ✅ **Storage Free:** No VM disk files in `/var/lib/libvirt/images/`
- ✅ **Disk Space:** More free space available

### Additional for Windows VMs:
- ✅ **fstab Clean:** No Windows mount entries in `/etc/fstab`
- ✅ **No Credentials:** `/etc/cifs-credentials` doesn't exist
- ✅ **No Mount Point:** `/mnt/windows-vm` doesn't exist

---

## 🎪 Ready for Fresh Installation!

Your system is now completely clean and ready for:
- ✨ Fresh VM installation
- 🔄 Different VM setup
- 💾 More free disk space for other projects
- 🚀 Clean slate to try new configurations

---

## 🚨 Emergency Recovery

**If you made a mistake and need to recover:**

```bash
# Restore fstab backup (Windows VMs only)
sudo cp /etc/fstab.backup.YYYYMMDD /etc/fstab

# Check what backups exist
ls /etc/fstab.backup.*
```

**If you deleted VM storage by accident:**
- Sorry, it's gone forever 😢
- This is why we warned you to backup first!
- Time to reinstall from your ISO

---

## 🏆 Congratulations!

You've successfully performed a complete VM cleanup! 🎉

**What you achieved:**
- 🧹 Completely removed VM and all traces
- 💾 Freed up significant disk space (real disk space, not just virtual!)
- 🔧 Cleaned Ubuntu system configuration (if Windows VM)
- 🎯 Prepared system for fresh installations
- 🚀 Became a VM management expert!

**Key things you learned:**
- 📊 How to check **real** vs **virtual** disk usage with `qemu-img info`
- 🔧 Why `sudo sh -c 'du -sh /var/lib/libvirt/images/*'` works better than wildcards
- 🐧 Ubuntu VMs are much simpler to clean up than Windows VMs
- 📈 QCOW2 thin provisioning saves tons of space!

**Next steps:** You can now create a brand new VM with a clean slate, or use the freed space for other projects!

---

*Made with ❤️ for clean systems and fresh starts!* ✨
