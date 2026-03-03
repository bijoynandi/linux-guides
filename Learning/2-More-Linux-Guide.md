# 🐧 Complete Linux/Ubuntu Guide for Beginners
*Your comprehensive reference to mastering Linux fundamentals*

---

## 📁 1. File System Hierarchy & Permissions (The Foundation)

### 🏗️ Understanding the File System Structure
Linux organizes everything in a tree structure starting from root (`/`). Think of it like a family tree where everything branches out from one main ancestor.

```
/               # 🏠 Root - The big boss, everything starts here
├── /bin        # 🔧 Essential user binaries (basic commands like ls, cp, mv)
├── /sbin       # ⚙️ System admin binaries (admin tools, usually need root)
├── /boot       # 🥾 Boot files and kernel (empty folder if separate boot partition)
├── /etc        # ⚙️ System configuration files (the brain of your system)
├── /home       # 🏠 User home directories, empty folder, gets mounted over (/home/username)
├── /root       # 👑 Root user's home directory (different from /home!)
├── /usr        # 📦 User programs and data (most software lives here)
├── /var        # 📊 Variable data (logs, cache, temporary files)
├── /tmp        # 🗑️ Temporary files (cleared on reboot)
├── /opt        # 📦 Optional software packages (third-party apps)
├── /proc       # 🔍 Virtual filesystem (live system information)
├── /sys        # 🔌 System and hardware information
├── /dev        # 🖥️ Device files (hardware represented as files)
├── /mnt        # 💾 Mount points for temporary filesystems
├── /media      # 📱 Mount points for removable media
└── /lib        # 📚 Essential shared libraries
```

**What this means for you:**
- 🏠 Your personal files live in `/home/yourusername`
- ⚙️ System settings are in `/etc`
- 📦 Programs are installed in `/usr` or `/opt`
- 🗑️ Temporary files go in `/tmp`

### 🔐 File Permissions (The Numbers Game)
Linux uses a permission system to control who can read, write, or execute files. Think of it like house keys - not everyone gets the same level of access.

```bash
# 📖 Understanding rwx permissions
r (read)    = 4  # Can view file contents or list directory
w (write)   = 2  # Can modify file contents or create/delete files in directory
x (execute) = 1  # Can run the file as a program or enter directory

# 🧮 The math
rwx = 4+2+1 = 7  # Full permissions (read + write + execute)
rw- = 4+2+0 = 6  # Read and write only
r-x = 4+0+1 = 5  # Read and execute only
r-- = 4+0+0 = 4  # Read only
-wx = 0+2+1 = 3  # Write and execute only
-w- = 0+2+0 = 2  # Write only
--x = 0+0+1 = 1  # Execute only
--- = 0+0+0 = 0  # No permissions

# 👥 Permission groups (Owner | Group | Others)
755 = rwxr-xr-x  # Owner: full access, Group & Others: read+execute
644 = rw-r--r--  # Owner: read+write, Group & Others: read only
600 = rw-------  # Owner: read+write, Group & Others: no access
777 = rwxrwxrwx  # Everyone: full access (⚠️ dangerous!)
```

**Common permission commands:**
```bash
# 📊 Check file permissions
ls -l filename              # Shows: -rw-r--r-- 1 user group 1024 date filename
ls -la                     # Show all files including hidden ones (starting with .)

# 🔧 Change permissions
chmod 755 filename         # Set specific permissions
chmod +x filename          # Add execute permission
chmod -w filename          # Remove write permission
chmod u+x filename         # Add execute for user only
chmod g-w filename         # Remove write for group only
chmod o=r filename         # Set others to read only

# 👤 Change ownership
chown user:group filename  # Change both user and group
chown user filename        # Change user only
chgrp group filename       # Change group only
```

### 🛡️ When to Use sudo (The Superpower)
`sudo` stands for "Super User DO" - it temporarily gives you admin powers. Use it wisely!

```bash
# ❌ NEVER use sudo for:
sudo pip install package        # Pollutes system Python, use virtual environments
sudo chmod 777 file            # Opens security holes
sudo rm -rf /                  # Deletes everything (system suicide)
sudo npm install -g package    # Use local installs or package managers

# ✅ USE sudo for:
sudo apt update                 # Updates package lists from repositories
sudo apt install package       # Installs system packages
sudo apt remove package        # Removes system packages
sudo systemctl start service   # Controls system services
sudo nano /etc/hosts           # Editing system configuration files
sudo mount /dev/sdb1 /mnt      # Mounting drives
sudo useradd username          # Adding system users
sudo passwd username           # Changing passwords
```

---

## 📦 2. Package Management (Software Installation)

### 🏪 The Four Horsemen of Package Management
Think of these as different app stores for Linux:

#### 1. **APT (Advanced Package Tool)** - The Traditional Way
```bash
# 🔄 Update package lists (like refreshing the app store)
sudo apt update

# ⬆️ Upgrade all installed packages
sudo apt upgrade

# 📦 Install a package
sudo apt install package_name

# 🔍 Search for packages
apt search keyword

# ℹ️ Get package information
apt show package_name

# 🗑️ Remove a package
sudo apt remove package_name

# 🧹 Remove package and its configuration files
sudo apt purge package_name

# 🧽 Clean up unused packages
sudo apt autoremove

# 📊 List installed packages
apt list --installed
```

#### 2. **Snap** - Universal Packages
```bash
# 📦 Install a snap package
sudo snap install package_name

# 📊 List installed snaps
snap list

# 🔍 Search for snaps
snap find keyword

# ℹ️ Get snap information
snap info package_name

# 🗑️ Remove a snap
sudo snap remove package_name

# ⬆️ Update all snaps
sudo snap refresh

# 🔄 Update specific snap
sudo snap refresh package_name
```

#### 3. **Flatpak** - Sandboxed Applications
```bash
# 🏪 Add Flathub repository (the main Flatpak store)
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# 📦 Install a Flatpak
flatpak install flathub package_name

# 🚀 Run a Flatpak
flatpak run package_name

# 📊 List installed Flatpaks
flatpak list

# 🔍 Search for Flatpaks
flatpak search keyword

# 🗑️ Remove a Flatpak
flatpak uninstall package_name
```

#### 4. **AppImage** - Portable Applications
```bash
# 📥 Download AppImage file
wget https://example.com/app.AppImage

# 🔧 Make it executable
chmod +x app.AppImage

# 🚀 Run it
./app.AppImage

# 📁 Optional: Install with AppImageLauncher for desktop integration
sudo apt install appimagelauncher
```

### 🎯 The Strategy (When to Use What)
1. **APT first** - Best system integration, official packages
2. **Snap** - When you need newer versions or the package isn't in APT
3. **Flatpak** - Great for GUI applications, good sandboxing
4. **AppImage** - When you need portability or quick testing

---

## ⚙️ 3. System Services & Process Management

### 🏃‍♂️ Understanding systemd (The System Manager)
systemd is like the manager of your Linux system - it starts, stops, and monitors all the services.

```bash
# 🚀 Start a service
sudo systemctl start service_name
# Example: sudo systemctl start apache2 (starts web server)

# 🛑 Stop a service
sudo systemctl stop service_name
# Example: sudo systemctl stop apache2 (stops web server)

# 🔄 Restart a service
sudo systemctl restart service_name
# Example: sudo systemctl restart networking (restarts network)

# ⏯️ Reload service configuration
sudo systemctl reload service_name
# Example: sudo systemctl reload nginx (reloads config without stopping)

# 🔄 Enable service to start on boot
sudo systemctl enable service_name
# Example: sudo systemctl enable ssh (SSH starts automatically)

# ❌ Disable service from starting on boot
sudo systemctl disable service_name
# Example: sudo systemctl disable apache2 (won't start automatically)

# 📊 Check service status
systemctl status service_name
# Shows: active, inactive, failed, with recent logs

# ✅ Check if service is running
systemctl is-active service_name
# Returns: active or inactive

# 🔍 Check if service is enabled
systemctl is-enabled service_name
# Returns: enabled or disabled

# 📋 List all services
systemctl list-units --type=service
# Shows all services and their status

# 🔍 List failed services
systemctl --failed
# Shows services that failed to start
```

### 🔍 Process Management (What's Running?)
```bash
# 📊 View running processes
ps aux                     # All processes with detailed info
ps -ef                     # All processes in full format
ps -u username             # Processes for specific user

# 🔍 Find specific processes
ps aux | grep process_name # Search for process by name
pgrep process_name         # Get process ID by name
pidof process_name         # Another way to get process ID

# 🔪 Kill processes
kill PID                   # Politely ask process to stop (SIGTERM)
kill -9 PID               # Force kill process (SIGKILL)
kill -15 PID              # Same as kill PID (SIGTERM)
killall process_name      # Kill all instances of a process
pkill process_name        # Kill processes by name pattern

# 📊 Real-time process monitoring
top                       # Basic process monitor
htop                      # Better, more colorful process monitor
iotop                     # I/O (disk) monitoring
nethogs                  # Network monitoring by process

# 🏃‍♂️ Run processes in background
command &                 # Run command in background
nohup command &           # Run command that survives logout
disown                    # Disconnect background job from terminal

# 📋 Job control
jobs                      # List background jobs
fg %1                     # Bring job 1 to foreground
bg %1                     # Send job 1 to background
```

---

## 🌐 4. Network Configuration & Troubleshooting

### 🔍 Network Basics
```bash
# 📡 Check network interfaces
ip addr show              # Modern way to show IP addresses
ip a                      # Short version
ifconfig                  # Traditional way (may need: sudo apt install net-tools)

# 🛤️ Check routing table
ip route show             # Modern way
ip r                      # Short version
route -n                  # Traditional way

# 🏓 Test connectivity
ping google.com           # Test if you can reach Google
ping -c 4 8.8.8.8        # Send only 4 packets to Google DNS
ping -c 1 192.168.1.1    # Test local router connection

# 🗺️ Trace network path
traceroute google.com     # Show route to destination
tracepath google.com     # Alternative tracing tool

# 🔍 DNS troubleshooting
nslookup google.com       # Look up DNS records
dig google.com            # More detailed DNS lookup
host google.com           # Simple hostname lookup

# 📊 Network statistics
netstat -tuln             # Show listening ports
netstat -tun              # Show all connections
ss -tuln                  # Modern replacement for netstat
lsof -i                   # Show network connections by process

# 📡 WiFi management
iwconfig                  # Show wireless interfaces
iwlist scan               # Scan for WiFi networks
nmcli device wifi list    # List WiFi networks (NetworkManager)
nmcli device wifi connect "SSID" password "password"  # Connect to WiFi
```

### 🔥 Firewall Management
```bash
# 🛡️ UFW (Uncomplicated Firewall) - Easy to use
sudo ufw enable           # Enable firewall
sudo ufw disable          # Disable firewall
sudo ufw status           # Check firewall status
sudo ufw status verbose   # Detailed status

# 🚪 Allow/deny specific ports
sudo ufw allow 22/tcp     # Allow SSH
sudo ufw allow 80/tcp     # Allow HTTP
sudo ufw allow 443/tcp    # Allow HTTPS
sudo ufw allow 21/tcp     # Allow FTP
sudo ufw deny 23/tcp      # Block Telnet (insecure)

# 🎯 Allow specific applications
sudo ufw allow ssh        # Allow SSH service
sudo ufw allow 'Apache Full'  # Allow Apache web server

# 📊 Advanced rules
sudo ufw allow from 192.168.1.0/24  # Allow from specific network
sudo ufw allow out 53     # Allow outgoing DNS
sudo ufw delete allow 80/tcp  # Remove a rule

# ⚙️ iptables (Advanced firewall)
sudo iptables -L          # List current rules
sudo iptables -F          # Flush all rules (⚠️ careful!)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
```

---

## 🖥️ 5. Display & Graphics (The Visual Experience)

### 🎨 Understanding Display Servers
```bash
# 🔍 Check current display server
echo $XDG_SESSION_TYPE    # Shows: x11 or wayland
loginctl show-session $(loginctl | grep $(whoami) | awk '{print $1}') -p Type

# 📊 Display information
xrandr                    # Show connected displays (X11)
xrandr --query            # Detailed display info
wlr-randr                 # Wayland equivalent (if available)
```

### 🎮 Graphics Drivers & Performance
```bash
# 🔍 Check graphics hardware
lspci | grep -i vga       # Show graphics cards
lspci | grep -i nvidia    # Show NVIDIA cards specifically
lshw -c display           # Detailed graphics info

# 🏎️ NVIDIA specific
nvidia-smi                # NVIDIA system info and running processes
nvidia-settings           # GUI for NVIDIA configuration
watch -n 1 nvidia-smi     # Monitor GPU usage in real-time

# 📊 Check OpenGL support
glxinfo | grep "OpenGL"   # OpenGL information
glxinfo | grep "direct"   # Check hardware acceleration
glxgears                  # Simple OpenGL test (FPS counter)

# 🎯 Install graphics drivers
sudo apt install nvidia-driver-470  # Install specific NVIDIA driver
sudo apt install mesa-vulkan-drivers # Install Mesa drivers for AMD/Intel
sudo ubuntu-drivers autoinstall     # Auto-install recommended drivers
```

### 🖥️ Multiple Monitor Setup
```bash
# 📊 List available displays
xrandr --listmonitors     # Show active monitors
xrandr --query            # Show all possible displays

# 🔧 Configure displays
xrandr --output HDMI1 --right-of eDP1     # Put HDMI monitor to the right
xrandr --output HDMI1 --left-of eDP1      # Put HDMI monitor to the left
xrandr --output HDMI1 --above eDP1        # Put HDMI monitor above
xrandr --output HDMI1 --same-as eDP1      # Mirror displays

# 📏 Set resolution
xrandr --output HDMI1 --mode 1920x1080    # Set specific resolution
xrandr --output HDMI1 --auto              # Use preferred resolution

# 🎚️ Brightness control
xrandr --output eDP1 --brightness 0.8     # Set brightness (0.0 to 1.0)
```

---

## 🌍 6. Environment Variables & Shell Configuration

### 🔧 Understanding the Shell Environment
```bash
# 🐚 Check current shell
echo $SHELL               # Shows path to current shell
echo $0                   # Shows current shell name
cat /etc/shells           # List all available shells

# 🌍 Important environment variables
echo $PATH                # Where shell looks for commands
echo $HOME                # Your home directory path
echo $USER                # Your username
echo $LANG                # Language and locale setting
echo $PWD                 # Current working directory
echo $OLDPWD              # Previous working directory

# 📊 View all environment variables
env                       # List all environment variables
printenv                  # Alternative command
set                       # Shows variables and functions

# 🔍 Find specific variables
env | grep PATH           # Find PATH variable
printenv | grep -i java   # Find Java-related variables
```

### ⚙️ Shell Configuration Files
```bash
# 📁 Bash configuration files (executed in order)
~/.bashrc                 # Interactive non-login shell configuration
~/.bash_profile           # Login shell configuration
~/.profile                # Shell-independent profile
~/.bash_logout            # Executed when bash login shell exits
/etc/bash.bashrc          # System-wide bash configuration
/etc/profile              # System-wide profile

# 🔧 Edit configuration files
nano ~/.bashrc            # Edit your bash configuration
source ~/.bashrc          # Reload configuration without restarting
. ~/.bashrc               # Alternative way to reload

# 🛤️ Managing PATH
export PATH="$PATH:/new/path"                    # Add to PATH temporarily
echo 'export PATH="$PATH:/new/path"' >> ~/.bashrc  # Add permanently
echo $PATH | tr ':' '\n'                        # Show PATH directories line by line
```

### 🎯 Useful Shell Aliases & Functions
```bash
# 📝 Common aliases (add to ~/.bashrc)
alias ll='ls -la'         # Detailed list
alias la='ls -la'         # Same as ll
alias l='ls -CF'          # Compact list
alias ..='cd ..'          # Go up one directory
alias ...='cd ../..'      # Go up two directories
alias grep='grep --color=auto'  # Colored grep
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'

# 🚀 Productivity aliases
alias h='history'         # Short history
alias c='clear'           # Clear screen
alias q='exit'            # Quick exit
alias reload='source ~/.bashrc'  # Reload configuration

# 📊 System information aliases
alias ports='netstat -tuln'      # Show listening ports
alias psg='ps aux | grep -v grep | grep -i -e VSZ -e'  # Process search
alias mkdir='mkdir -pv'          # Create parent dirs and be verbose
alias df='df -H'                 # Human readable disk usage
alias du='du -ch'                # Human readable directory usage
```

---

## 📊 7. Log Files & System Monitoring

### 🗂️ Where Linux Keeps Its Secrets
```bash
# 📋 Important log locations
/var/log/syslog           # General system messages
/var/log/auth.log         # Authentication and authorization logs
/var/log/kern.log         # Kernel messages
/var/log/dmesg            # Boot messages
/var/log/apache2/         # Apache web server logs
/var/log/nginx/           # Nginx web server logs
/var/log/mysql/           # MySQL database logs
/var/log/apt/             # Package management logs
/var/log/cron.log         # Scheduled task logs

# 📖 Viewing logs
tail -f /var/log/syslog           # Follow log in real-time
tail -n 50 /var/log/auth.log      # Show last 50 lines
head -n 20 /var/log/syslog        # Show first 20 lines
cat /var/log/dmesg                # View entire file
less /var/log/syslog              # View with pager (q to quit)
grep "error" /var/log/syslog      # Search for errors
```

### 📰 systemd Journal (Modern Logging)
```bash
# 📊 View journal
journalctl                        # Show all journal entries
journalctl -f                     # Follow journal in real-time
journalctl -u service_name        # Show logs for specific service
journalctl -p err                 # Show only error messages
journalctl --since "1 hour ago"   # Show recent logs
journalctl --since "2023-01-01"   # Show logs since date
journalctl --until "2023-01-02"   # Show logs until date

# 🔍 Advanced journal queries
journalctl -b                     # Show logs from current boot
journalctl -b -1                  # Show logs from previous boot
journalctl -k                     # Show kernel messages
journalctl --disk-usage           # Show disk usage of journal
journalctl --vacuum-size=100M     # Limit journal size to 100MB
```

### 📊 System Monitoring Commands
```bash
# 💾 Disk usage
df -h                     # Show disk free space (human readable)
df -i                     # Show inode usage
du -sh /path/to/dir      # Show directory size
du -ah /path | sort -rh | head -20  # Show 20 largest files/dirs
ncdu                     # Interactive disk usage analyzer
lsblk                    # List block devices
fdisk -l                 # Show disk partitions

# 🧠 Memory usage
free -h                   # Show RAM usage (human readable)
free -m                   # Show RAM usage in MB
cat /proc/meminfo         # Detailed memory information
vmstat 1 5               # Show virtual memory statistics (1 sec, 5 times)

# 🏃‍♂️ CPU and system load
uptime                   # System uptime and load average
w                        # Who's logged in and what they're doing
who                      # Show logged in users
last                     # Show recent login history
lscpu                    # Show CPU information
nproc                    # Show number of processing cores

# 📊 Process monitoring
top                      # Real-time process viewer
htop                     # Better version of top (install: sudo apt install htop)
atop                     # Advanced system monitor
iotop                    # I/O monitoring (sudo apt install iotop)
nethogs                  # Network monitoring by process

# 🔧 System information
uname -a                 # System information
hostnamectl              # System hostname and info
timedatectl              # System time and date info
systemctl status         # Overall system status
```

---

## 🔐 8. SSH & Remote Access

### 🔑 SSH Mastery (Secure Shell)
```bash
# 🚀 Basic SSH connection
ssh username@hostname             # Connect to remote server
ssh username@192.168.1.100       # Connect using IP address
ssh -p 2222 username@hostname    # Connect using custom port

# 🔐 Key-based authentication (more secure)
ssh-keygen -t rsa -b 4096        # Generate RSA key pair (4096 bits)
ssh-keygen -t ed25519            # Generate Ed25519 key (faster, modern)
ssh-copy-id username@hostname    # Copy public key to remote server
ssh-copy-id -i ~/.ssh/id_rsa.pub username@hostname  # Copy specific key

# 🔍 SSH key management
ls -la ~/.ssh/                   # List SSH keys
ssh-add -l                       # List loaded keys
ssh-add ~/.ssh/id_rsa           # Add key to SSH agent
ssh-add -D                      # Remove all keys from agent
```

### ⚙️ SSH Configuration
```bash
# 📝 SSH client config file (~/.ssh/config)
Host myserver
    HostName 192.168.1.100
    User myuser
    Port 2222
    IdentityFile ~/.ssh/my_private_key

Host webserver
    HostName web.example.com
    User admin
    ForwardX11 yes
    LocalForward 8080 localhost:80

# Then just use: ssh myserver

# 🔧 SSH server configuration (/etc/ssh/sshd_config)
sudo nano /etc/ssh/sshd_config
# Important settings:
# Port 22                    # Change default port for security
# PasswordAuthentication no  # Disable password login (keys only)
# PubkeyAuthentication yes   # Enable key-based authentication
# PermitRootLogin no        # Disable root login

# 🔄 Restart SSH service after changes
sudo systemctl restart ssh
```

### 📁 Secure File Transfer
```bash
# 📤 SCP (Secure Copy)
scp file.txt user@host:/path/to/destination      # Copy file to remote
scp user@host:/path/to/file.txt ./               # Copy file from remote
scp -r folder/ user@host:/path/to/destination    # Copy directory recursively
scp -P 2222 file.txt user@host:/path/            # Use custom port

# 🔄 SFTP (Secure File Transfer Protocol)
sftp user@hostname
# SFTP commands:
# put local_file.txt        # Upload file
# get remote_file.txt       # Download file
# ls                        # List remote files
# lls                       # List local files
# cd /path                  # Change remote directory
# lcd /path                 # Change local directory
# quit                      # Exit SFTP

# 📊 rsync (Advanced file synchronization)
rsync -avz /local/path/ user@host:/remote/path/  # Sync directories
rsync -avz --delete /local/ user@host:/remote/   # Sync and delete extra files
rsync -avz --exclude="*.tmp" /local/ user@host:/remote/  # Exclude patterns
```

---

## 📝 9. Text Processing & Command Line Mastery

### ✂️ Stream Editors & Text Processing
```bash
# 🔄 sed (Stream Editor) - Find and replace master
sed 's/old/new/' file.txt           # Replace first occurrence per line
sed 's/old/new/g' file.txt          # Replace all occurrences (global)
sed -i 's/old/new/g' file.txt       # Edit file in-place
sed '1,5d' file.txt                 # Delete lines 1-5
sed '/pattern/d' file.txt           # Delete lines containing pattern
sed -n '10,20p' file.txt           # Print only lines 10-20
sed 's/^/> /' file.txt             # Add "> " to beginning of each line

# 🔍 awk (Pattern Processing Language)
awk '{print $1}' file.txt           # Print first column
awk '{print $NF}' file.txt          # Print last column
awk '/pattern/ {print $0}' file.txt # Print lines matching pattern
awk -F: '{print $1}' /etc/passwd    # Use : as field separator
awk '{sum += $1} END {print sum}' file.txt  # Sum first column
awk 'length($0) > 80' file.txt      # Print lines longer than 80 characters

# 🔍 grep (Global Regular Expression Print)
grep "pattern" file.txt             # Find pattern in file
grep -r "pattern" /path/            # Recursive search in directory
grep -i "pattern" file.txt          # Case-insensitive search
grep -v "pattern" file.txt          # Invert match (lines NOT containing pattern)
grep -n "pattern" file.txt          # Show line numbers
grep -c "pattern" file.txt          # Count matching lines
grep -A 5 "pattern" file.txt        # Show 5 lines after match
grep -B 5 "pattern" file.txt        # Show 5 lines before match
grep -C 5 "pattern" file.txt        # Show 5 lines before and after match
```

### 🔗 Command Chaining & Redirection
```bash
# 📤 Output redirection
command > file.txt              # Redirect stdout to file (overwrite)
command >> file.txt             # Append stdout to file
command 2> error.log            # Redirect stderr to file
command 2>&1                    # Redirect stderr to stdout
command &> all.log              # Redirect both stdout and stderr
command > /dev/null             # Discard output (black hole)

# 📥 Input redirection
command < input.txt             # Read input from file
command << EOF                  # Here document (multi-line input)
line 1
line 2
EOF

# 🔗 Command chaining
command1 && command2            # Run command2 only if command1 succeeds
command1 || command2            # Run command2 only if command1 fails
command1 ; command2             # Run command2 regardless of command1 result
command1 | command2             # Pipe: output of command1 becomes input to command2
command1 | tee file.txt         # Pipe to command and save to file simultaneously
```

### 🔧 Advanced Text Tools
```bash
# 📊 cut (Extract columns)
cut -d: -f1 /etc/passwd         # Extract first field using : as delimiter
cut -c1-5 file.txt              # Extract characters 1-5
cut -f2,4 file.txt              # Extract fields 2 and 4

# 🔄 sort (Sort lines)
sort file.txt                   # Sort alphabetically
sort -n file.txt                # Sort numerically
sort -r file.txt                # Sort in reverse
sort -k2 file.txt               # Sort by second field
sort -u file.txt                # Sort and remove duplicates

# 📋 uniq (Remove duplicates)
uniq file.txt                   # Remove consecutive duplicate lines
uniq -c file.txt                # Count occurrences
uniq -d file.txt                # Show only duplicate lines
uniq -u file.txt                # Show only unique lines

# 🔍 wc (Word Count)
wc file.txt                     # Count lines, words, characters
wc -l file.txt                  # Count lines only
wc -w file.txt                  # Count words only
wc -c file.txt                  # Count characters only
```

---

## 🗂️ 10. Advanced File Operations

### 🔍 Find Command Mastery
```bash
# 📁 Finding files by name
find /path -name "*.txt"         # Find all .txt files
find /path -name "file*"         # Find files starting with "file"
find /path -iname "*.TXT"        # Case-insensitive search
find . -name "*.log" -delete     # Find and delete .log files

# 🎯 Finding by file type
find /path -type f               # Find regular files only
find /path -type d               # Find directories only
find /path -type l               # Find symbolic links only

# 📏 Finding by size
find /path -size +100M           # Find files larger than 100MB
find /path -size -1k             # Find files smaller than 1KB
find /path -size 1G              # Find files exactly 1GB

# 📅 Finding by time
find /path -mtime -7             # Modified in last 7 days
find /path -mtime +30            # Modified more than 30 days ago
find /path -atime -1             # Accessed in last 1 day
find /path -ctime -1             # Changed in last 1 day

# 🔐 Finding by permissions
find /path -perm 755             # Find files with exact permissions
find /path -perm -755            # Find files with at least these permissions
find /path -perm /755            # Find files with any of these permissions
find /path -user username        # Find files owned by specific user
find /path -group groupname      # Find files owned by specific group

# 🔧 Execute commands on found files
find /path -name "*.txt" -exec chmod 644 {} \;    # Change permissions
find /path -name "*.log" -exec rm {} \;           # Delete found files
# 🔍 locate (Lightning Fast Search)
sudo updatedb                    # Update locate database (run daily via cron)
locate filename                  # Find files by name (very fast)
locate -i filename               # Case-insensitive search
locate -c filename               # Count matches only
locate -r "pattern"              # Use regex pattern

# 🔍 which & whereis (Find Commands)
which command                    # Find location of command
whereis command                  # Find binary, source, and manual pages
type command                     # Show command type (built-in, alias, or file)
```

### 📦 Archive & Compression
```bash
# 📁 tar (Tape Archive) - The Swiss Army Knife
tar -czf archive.tar.gz directory/     # Create compressed archive (.tar.gz)
tar -czf archive.tar.gz file1 file2    # Archive multiple files
tar -xzf archive.tar.gz                # Extract compressed archive
tar -xzf archive.tar.gz -C /path/      # Extract to specific directory
tar -tzf archive.tar.gz                # List contents without extracting
tar -xzf archive.tar.gz file.txt       # Extract specific file only

# 📁 Different compression types
tar -cf archive.tar directory/         # Create uncompressed archive
tar -czf archive.tar.gz directory/     # Create gzip compressed archive
tar -cjf archive.tar.bz2 directory/    # Create bzip2 compressed archive
tar -cJf archive.tar.xz directory/     # Create xz compressed archive

# 📦 zip archives
zip -r archive.zip directory/          # Create zip archive
zip -r archive.zip file1 file2         # Zip multiple files
unzip archive.zip                       # Extract zip archive
unzip archive.zip -d /path/             # Extract to specific directory
unzip -l archive.zip                    # List contents
unzip -q archive.zip                    # Quiet extraction
zip -u archive.zip newfile.txt          # Update archive with new file

# 🗜️ Other compression tools
gzip file.txt                           # Compress file (creates file.txt.gz)
gunzip file.txt.gz                      # Decompress gzip file
bzip2 file.txt                          # Compress with bzip2
bunzip2 file.txt.bz2                    # Decompress bzip2 file
xz file.txt                             # Compress with xz
unxz file.txt.xz                        # Decompress xz file
```

---

## 🎯 11. Process Control & Job Management

### 🏃‍♂️ Background Processing
```bash
# 🚀 Running commands in background
command &                       # Run command in background
nohup command &                  # Run command that survives terminal closure
disown %1                       # Remove job from shell's job table
screen -S session_name          # Create persistent session
tmux new-session -s work        # Create tmux session

# 📊 Job control
jobs                            # List active jobs
jobs -l                         # List jobs with process IDs
fg                              # Bring most recent job to foreground
fg %2                           # Bring job 2 to foreground
bg                              # Send most recent job to background
bg %2                           # Send job 2 to background
kill %1                         # Kill job 1
```

### 🔄 Process Signals
```bash
# 📡 Common signals
kill -l                         # List all available signals
kill -TERM PID                  # Terminate process gracefully (default)
kill -KILL PID                  # Force kill process (cannot be caught)
kill -STOP PID                  # Stop (pause) process
kill -CONT PID                  # Continue stopped process
kill -HUP PID                   # Hang up (reload configuration)
kill -USR1 PID                  # User-defined signal 1
kill -USR2 PID                  # User-defined signal 2

# 🎯 Process priority
nice -n 10 command              # Run command with lower priority
nice -n -10 command             # Run command with higher priority
renice 5 PID                    # Change priority of running process
```

---

## 📅 12. System Scheduling & Automation

### ⏰ Cron Jobs (Scheduled Tasks)
```bash
# 📝 Edit crontab
crontab -e                      # Edit current user's crontab
crontab -l                      # List current user's crontab
crontab -r                      # Remove current user's crontab
sudo crontab -e                 # Edit root's crontab

# 📊 Cron format: minute hour day month day_of_week command
# 0 2 * * * /path/to/script.sh           # Run daily at 2 AM
# 30 14 * * 1 /path/to/script.sh         # Run Mondays at 2:30 PM
# 0 0 1 * * /path/to/script.sh           # Run first day of each month
# */15 * * * * /path/to/script.sh        # Run every 15 minutes
# 0 9-17 * * 1-5 /path/to/script.sh      # Run hourly, 9-5, weekdays only

# 📁 System cron directories
/etc/cron.daily/                # Scripts run daily
/etc/cron.hourly/               # Scripts run hourly
/etc/cron.weekly/               # Scripts run weekly
/etc/cron.monthly/              # Scripts run monthly
```

### ⚡ at (One-time Scheduled Tasks)
```bash
# 🕐 Schedule one-time tasks
at 14:30                        # Schedule task for 2:30 PM today
at now + 1 hour                 # Schedule task for 1 hour from now
at 9:00 AM tomorrow             # Schedule task for tomorrow morning
at midnight                     # Schedule task for midnight

# 📊 Manage at jobs
atq                            # List scheduled jobs
atrm 1                         # Remove job number 1
batch                          # Run when system load is low
```

---

## 🔒 13. User & Group Management

### 👤 User Management
```bash
# 👨‍💼 Create users
sudo useradd username           # Create new user (basic)
sudo useradd -m username        # Create user with home directory
sudo useradd -m -s /bin/bash username  # Create user with specific shell
sudo adduser username           # Interactive user creation (Ubuntu)

# 🔑 Password management
sudo passwd username            # Set password for user
passwd                          # Change your own password
sudo passwd -l username         # Lock user account
sudo passwd -u username         # Unlock user account
sudo passwd -d username         # Delete user password

# 🗑️ Remove users
sudo userdel username           # Delete user (keep home directory)
sudo userdel -r username        # Delete user and home directory
sudo deluser username           # Ubuntu alternative

# ℹ️ User information
id username                     # Show user ID and groups
whoami                         # Show current username
w                              # Show who's logged in
last                           # Show login history
finger username                # Show user information (if installed)
```

### 👥 Group Management
```bash
# 🏗️ Create groups
sudo groupadd groupname         # Create new group
sudo groupadd -g 1001 groupname # Create group with specific GID

# 👨‍👩‍👧‍👦 Manage group membership
sudo usermod -a -G groupname username    # Add user to group
sudo gpasswd -a username groupname       # Alternative way to add user
sudo gpasswd -d username groupname       # Remove user from group
sudo usermod -G group1,group2 username   # Set user's groups (replaces all)

# 🗑️ Remove groups
sudo groupdel groupname         # Delete group

# ℹ️ Group information
groups username                 # Show user's groups
getent group                   # List all groups
cat /etc/group                 # View group file directly
```

---

## 🛠️ 14. System Maintenance & Updates

### 🔄 System Updates
```bash
# 📦 APT package management
sudo apt update                 # Update package lists
sudo apt upgrade                # Upgrade installed packages
sudo apt dist-upgrade           # Upgrade with dependency resolution
sudo apt full-upgrade           # Full system upgrade
sudo apt autoremove             # Remove unused packages
sudo apt autoclean              # Clean package cache

# 🔍 Package information
apt list --upgradable           # Show packages that can be upgraded
apt show package_name           # Show detailed package information
apt-cache search keyword        # Search for packages
dpkg -l                        # List all installed packages
dpkg -L package_name           # List files installed by package

# 📊 System version information
lsb_release -a                 # Show Ubuntu version info
cat /etc/os-release            # Show OS release information
uname -a                       # Show kernel information
hostnamectl                    # Show system information
```

### 🧹 System Cleanup
```bash
# 🗑️ Clean temporary files
sudo rm -rf /tmp/*             # Clear temporary files
sudo rm -rf /var/tmp/*         # Clear variable temporary files
sudo apt autoclean            # Clean package cache
sudo apt autoremove           # Remove unused packages

# 📊 Disk space analysis
df -h                          # Show disk usage
du -sh /*                      # Show directory sizes in root
du -sh ~/.*                    # Show hidden directory sizes in home
ncdu /                         # Interactive disk usage analyzer

# 🗂️ Log cleanup
sudo journalctl --vacuum-time=7d    # Keep only 7 days of journal logs
sudo journalctl --vacuum-size=100M  # Limit journal size to 100MB
sudo find /var/log -name "*.log" -mtime +30 -delete  # Delete old log files
```

---

## 🎓 15. Pro Tips & Advanced Techniques

### ⌨️ Keyboard Shortcuts & Efficiency
```bash
# 🔄 History navigation
Ctrl + R                       # Reverse search through command history
Ctrl + P                       # Previous command in history
Ctrl + N                       # Next command in history
!!                            # Run last command
!n                            # Run command number n from history
!string                       # Run last command starting with string
^old^new                      # Replace 'old' with 'new' in last command

# ✏️ Command line editing
Ctrl + A                       # Move cursor to beginning of line
Ctrl + E                       # Move cursor to end of line
Ctrl + U                       # Delete from cursor to beginning of line
Ctrl + K                       # Delete from cursor to end of line
Ctrl + W                       # Delete word before cursor
Alt + F                        # Move forward one word
Alt + B                        # Move backward one word

# 🎯 Process control
Ctrl + C                       # Kill current process
Ctrl + Z                       # Suspend current process
Ctrl + D                       # Exit shell or end input
Ctrl + S                       # Pause output
Ctrl + Q                       # Resume output
```

### 🔧 Useful Aliases & Functions
```bash
# 📝 Add these to ~/.bashrc for productivity
alias ll='ls -la'
alias la='ls -la'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias ~='cd ~'
alias h='history'
alias c='clear'
alias q='exit'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'
alias df='df -h'
alias du='du -h'
alias free='free -h'
alias ps='ps auxf'
alias mkdir='mkdir -pv'
alias wget='wget -c'
alias reload='source ~/.bashrc'

# 🚀 Useful functions
function mkcd() { mkdir -p "$1" && cd "$1"; }  # Create directory and cd into it
function extract() {                           # Extract any archive
    if [ -f $1 ]; then
        case $1 in
            *.tar.bz2)   tar xjf $1     ;;
            *.tar.gz)    tar xzf $1     ;;
            *.bz2)       bunzip2 $1     ;;
            *.rar)       unrar x $1     ;;
            *.gz)        gunzip $1      ;;
            *.tar)       tar xf $1      ;;
            *.tbz2)      tar xjf $1     ;;
            *.tgz)       tar xzf $1     ;;
            *.zip)       unzip $1       ;;
            *.Z)         uncompress $1  ;;
            *.7z)        7z x $1        ;;
            *)           echo "Don't know how to extract '$1'..." ;;
        esac
    else
        echo "'$1' is not a valid file!"
    fi
}

# 📊 System information function
function sysinfo() {
    echo "📊 System Information:"
    echo "🖥️  Hostname: $(hostname)"
    echo "👤 User: $(whoami)"
    echo "🐧 OS: $(lsb_release -d | cut -f2)"
    echo "🔧 Kernel: $(uname -r)"
    echo "⏰ Uptime: $(uptime -p)"
    echo "🧠 Memory: $(free -h | grep '^Mem:' | awk '{print $3 "/" $2}')"
    echo "💾 Disk: $(df -h / | tail -1 | awk '{print $3 "/" $2 " (" $5 ")"}')"
    echo "🔥 CPU: $(nproc) cores"
    echo "🌡️  Load: $(uptime | grep -ohe 'load average[s:][: ].*' | awk '{ print $3 $4 $5 }')"
}
```

### 🔍 Advanced Search & Find Techniques
```bash
# 🎯 Complex find operations
find . -name "*.txt" -not -path "./backup/*"  # Exclude backup directory
find . -name "*.log" -size +10M -exec ls -lh {} \;  # Find large log files
find . -type f -name "*.py" -exec grep -l "import os" {} \;  # Find Python files importing os
find . -type f -perm /u+x  # Find executable files
find . -type f -newer reference_file  # Find files newer than reference

# 🔍 Advanced grep patterns
grep -E "pattern1|pattern2" file.txt      # Multiple patterns (OR)
grep -E "pattern1.*pattern2" file.txt     # Both patterns on same line
grep -P "\d{3}-\d{2}-\d{4}" file.txt     # Perl regex (social security numbers)
grep -B3 -A3 "error" /var/log/syslog     # Show 3 lines before and after matches
```

---

## 🔧 16. Text Editors & Configuration

### 📝 nano (Beginner-Friendly Editor)
```bash
# 🚀 Basic nano usage
nano filename                   # Open file in nano
nano -w filename               # Disable word wrapping
nano +10 filename              # Open file at line 10

# ⌨️ nano keyboard shortcuts
Ctrl + X                       # Exit nano
Ctrl + O                       # Save file (WriteOut)
Ctrl + R                       # Read file (Insert file)
Ctrl + W                       # Search
Ctrl + \                       # Replace
Ctrl + G                       # Help
Ctrl + K                       # Cut line
Ctrl + U                       # Paste line
Ctrl + _                       # Go to line number
```

### 🎯 vim (Advanced Editor)
```bash
# 🚀 Basic vim usage
vim filename                   # Open file in vim
vim +10 filename              # Open file at line 10
vim -R filename               # Open file read-only

# 🎮 vim modes
i                             # Insert mode
Esc                           # Normal mode
:                             # Command mode
v                             # Visual mode

# 💾 Save and exit
:w                            # Save file
:q                            # Quit
:wq                           # Save and quit
:q!                           # Quit without saving
:x                            # Save and quit (same as :wq)
```

---

## 🌐 17. Network Services & Configuration

### 🔧 Network Configuration Files
```bash
# 📁 Important network files
/etc/hostname                  # System hostname
/etc/hosts                     # Static hostname resolution
/etc/resolv.conf              # DNS configuration
/etc/network/interfaces       # Network interfaces (Debian/Ubuntu)
/etc/netplan/                 # Netplan configuration (Ubuntu 18.04+)
/etc/NetworkManager/          # NetworkManager configuration

# 🔧 Edit network configuration
sudo nano /etc/hosts          # Add local hostname mappings
sudo nano /etc/resolv.conf    # Configure DNS servers
sudo netplan apply            # Apply netplan configuration
sudo systemctl restart networking  # Restart networking service
```

### 🌐 Common Network Services
```bash
# 🌐 Web servers
sudo systemctl start apache2   # Start Apache web server
sudo systemctl start nginx     # Start Nginx web server
curl http://localhost          # Test local web server
wget -O- http://localhost      # Download webpage

# 🗄️ Database servers
sudo systemctl start mysql     # Start MySQL database
sudo systemctl start postgresql  # Start PostgreSQL database
sudo mysql -u root -p          # Connect to MySQL as root

# 🔐 SSH server
sudo systemctl start ssh       # Start SSH server
sudo systemctl enable ssh      # Enable SSH on boot
sudo nano /etc/ssh/sshd_config # Configure SSH server
```

---

## 🎯 18. The Linux Philosophy & Best Practices

### 🧠 The Linux Mindset
1. **🗂️ Everything is a File**
   - Devices, processes, network connections - all represented as files
   - This is why `/dev/null` exists (the black hole file)
   - `/proc` contains running process information as files

2. **🔒 Principle of Least Privilege**
   - Don't run as root unless absolutely necessary
   - Use sudo only when needed for system administration
   - Create separate user accounts for different purposes

3. **🧩 Modularity & Composability**
   - Small tools that do one thing well
   - Combine tools with pipes for complex operations
   - Write shell scripts to automate repetitive tasks

4. **📝 Configuration is Text**
   - Most configuration files are plain text
   - Version control your important config files
   - Learn at least one text editor well

### 💡 Best Practices
```bash
# 📚 Always read documentation
man command                    # Manual pages
command --help                 # Quick help
info command                   # Info pages (detailed)

# 🔍 Before installing, search
apt search package_name        # Search before installing
apt show package_name          # Get package details

# 🛡️ Security practices
sudo apt update && sudo apt upgrade  # Keep system updated
sudo ufw enable                # Enable firewall
chmod 600 ~/.ssh/id_rsa       # Secure SSH private keys
sudo passwd -l root            # Lock root account (Ubuntu does this by default)

# 📦 Backup important files
tar -czf backup.tar.gz ~/.bashrc ~/.profile ~/.vimrc  # Backup config files
rsync -avz /home/user/ /backup/location/  # Backup home directory
```

---

## 🎓 19. Essential Commands Reference

### 📋 Must-Know Commands
```bash
# 🗂️ File and directory operations
ls, cd, pwd, mkdir, rmdir, rm, cp, mv, find, locate, which, chmod, chown

# 📝 Text processing
cat, less, more, head, tail, grep, sed, awk, sort, uniq, cut, wc

# 📊 System information
ps, top, htop, df, du, free, uname, whoami, id, groups, lscpu, lsblk

# 🌐 Network
ping, wget, curl, ssh, scp, rsync, netstat, ss, nslookup, dig

# 📦 Package management
apt, dpkg, snap, flatpak (Ubuntu/Debian specific)

# ⚙️ System control
systemctl, service, sudo, su, crontab, at, jobs, fg, bg, nohup

# 🗜️ Archive and compression
tar, zip, unzip, gzip, gunzip, bzip2, bunzip2
```

### 🔧 System Administration Commands
```bash
# 👤 User management
useradd, userdel, usermod, passwd, su, sudo, groups, id

# 🔒 Permissions and ownership
chmod, chown, chgrp, umask

# 📊 Process management
ps, top, htop, kill, killall, pgrep, pkill, jobs, bg, fg

# 🌐 Network administration
ifconfig, ip, route, netstat, ss, iptables, ufw

# 💾 Storage management
fdisk, mount, umount, df, du, lsblk, blkid

# 📋 Log management
journalctl, tail, head, grep, less
```

---

## 🎯 20. Troubleshooting Guide

### 🔍 Common Problems & Solutions

#### 🚫 Permission Denied Errors
```bash
# Problem: Permission denied when running command
# Solution: Check file permissions and ownership
ls -la filename
chmod +x filename              # Add execute permission
sudo chown user:group filename # Change ownership
```

#### 🌐 Network Connection Issues
```bash
# Problem: Can't connect to internet
# Solutions:
ping 8.8.8.8                  # Test basic connectivity
nslookup google.com           # Test DNS resolution
sudo systemctl restart NetworkManager  # Restart network service
cat /etc/resolv.conf          # Check DNS configuration
```

#### 💾 Disk Space Issues
```bash
# Problem: Disk full errors
# Solutions:
df -h                         # Check disk usage
du -sh /* | sort -h           # Find largest directories
sudo apt autoremove           # Remove unused packages
sudo apt autoclean            # Clean package cache
sudo journalctl --vacuum-time=7d  # Clean old logs
```

#### 🔧 Service Won't Start
```bash
# Problem: Service fails to start
# Solutions:
systemctl status service_name  # Check service status
journalctl -u service_name     # Check service logs
sudo systemctl enable service_name  # Enable service
sudo systemctl daemon-reload   # Reload systemd configuration
```

---

## 🎉 Conclusion

Congratulations! 🎊 You now have a comprehensive guide to Linux/Ubuntu fundamentals. Remember:

### 🚀 Your Learning Journey
1. **Start Small**: Master basic commands before moving to advanced topics
2. **Practice Daily**: Use the terminal for everyday tasks
3. **Read Documentation**: `man` pages are your best friend
4. **Join Communities**: Ask questions, help others, contribute back
5. **Experiment Safely**: Use virtual machines for testing

### 📚 Next Steps
- Set up a home lab for practice
- Learn shell scripting (bash)
- Explore system administration
- Try different Linux distributions
- Contribute to open source projects

### 🛡️ Safety Reminders
- Always backup important data
- Test commands in safe environments first
- Use `sudo` responsibly
- Keep your system updated
- Follow the principle of least privilege

**Remember**: Linux mastery comes from understanding concepts and practicing regularly. You don't need to memorize everything - just know where to look and how to think about problems the Linux way! 🐧

*Happy Linux journey! 🎯*
