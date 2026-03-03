# The Ultimate SSH Reference Manual 🔐
*Your Complete Guide to SSH Mastery*

---

## Table of Contents 📚
1. [What is SSH?](#what-is-ssh)
2. [SSH Architecture](#ssh-architecture)
3. [Installation & Setup](#installation--setup)
4. [Basic SSH Commands](#basic-ssh-commands)
5. [SSH Keys (Passwordless Login)](#ssh-keys-passwordless-login)
6. [Advanced SSH Features](#advanced-ssh-features)
7. [Security & Best Practices](#security--best-practices)
8. [Troubleshooting](#troubleshooting)
9. [SSH Configuration Files](#ssh-configuration-files)
10. [Common Use Cases](#common-use-cases)

---

## What is SSH? 🤔

**SSH (Secure Shell)** is like having a **secure telephone line** to another computer. It's the standard way to:
- 🖥️ Control remote computers
- 📁 Transfer files securely
- 🔒 Encrypt all communication
- 🌍 Manage servers across the globe

### Key Benefits:
- ✅ **Encrypted**: All data is scrambled during transmission
- ✅ **Authenticated**: Proves you are who you say you are
- ✅ **Portable**: Works on any Unix-like system (Linux, macOS, etc.)
- ✅ **Versatile**: Remote shells, file transfers, tunneling, and more

---

## SSH Architecture 🏗️

### The Players:
```
┌─────────────────┐         ┌─────────────────┐
│   SSH CLIENT    │ ◄────► │   SSH SERVER    │
│  (Your System)  │         │ (Remote System) │
│                 │         │                 │
│ • ssh command   │         │ • sshd daemon   │
│ • Makes calls   │         │ • Receives calls│
└─────────────────┘         └─────────────────┘
```

### Connection Process:
1. 📞 **Client**: "Hello server, I want to connect!"
2. 🔑 **Server**: "Hi! Here's my identity (host key)"
3. 🔍 **Client**: "Let me verify you're the real server..."
4. ✅ **Client**: "Verified! I'm user 'bijoy'"
5. 🔐 **Server**: "Prove it! Password or key?"
6. ✅ **Authentication**: Password/key verification
7. 🎉 **Connected**: Secure shell session established!

---

## Installation & Setup ⚙️

### Installing SSH Client (Usually Already There):
```bash
# Check if you have SSH client
which ssh
# Should show: /usr/bin/ssh

# If missing (rare):
sudo apt install openssh-client
```

### Installing SSH Server (To Accept Connections):
```bash
# Install SSH server
sudo apt install openssh-server (In case of Arch sudo pacman -S openssh)

# Start the service
sudo systemctl start ssh

# Enable auto-start on boot
sudo systemctl enable ssh

# Check status
sudo systemctl status ssh
```

### GUI Method (GNOME):
```
Settings → System → Remote Desktop → SSH Network Access → ON
```
*Boom! SSH server installed and running!* 🚀

---

## Basic SSH Commands 💻

### Basic Connection:
```bash
# Know the details of the hosting server (also need the login password)
whoami
ip addr show
hostname -I
# Connect to remote system
ssh username@hostname
ssh username@ip.address
ssh bijoy@192.168.122.X (Type yes if connecting to server for the first time to add the fingerprint to the ~/.ssh/known_hosts file)
# Connect with specific port
ssh -p 2222 username@hostname
# Connect with verbose output (debugging)
ssh -v username@hostname
```

### ⚠️ Add this if firewall blocks the port (Port 22) by default (Like openSUSE for example blocks ssh by default) ⚠️
```bash
sudo firewall-cmd --state (Check current firewall status)
sudo firewall-cmd --list-services (Check current firewall status)
sudo firewall-cmd --permanent --add-service=ssh  # Add SSH service permanently
sudo firewall-cmd --reload  # Reload firewall rules
sudo firewall-cmd --list-services  # Verify SSH is now allowed
```

### ⚠️ Add this if custom port (like Port 2222) is in place of the default Port 22 ⚠️
```bash
sudo nano /etc/ssh/sshd_config  # Edit SSH config to use non-standard port  # Change: Port 2222
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports  # To see the port it get added
```

### Fedora and openSUSE Tumbleweed specific commands after changing port number 🤠 🦎
### Allow port 2222 in SELinux:
```bash
sudo semanage port -a -t ssh_port_t -p tcp 2222 (Required on both Fedora and openSUSE Tumbleweed)
sudo semanage port -l | grep ssh (Check if it\'s added or not)
sudo systemctl restart sshd
sudo systemctl status sshd
```

### ℹ️ ⚠️ Do not edit the sshd_config file directly in the /etc/ssh if /etc/ssh/sshd_config.d directory is avialable
```bash
# Create your SSH config file (name it anything ending in .conf):
sudo nano /etc/ssh/sshd_config.d/custom.conf

# Add:
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

### Proper SSH Configuration Workflow:
```bash
 1. Edit SSH config
sudo nano /etc/ssh/sshd_config.d/custom.conf

# 2. Test syntax
sudo sshd -t

# 3. Restart SSH service
sudo systemctl restart sshd

# 4. Check status
sudo systemctl status sshd
```

### Remote Command Execution:
```bash
# Run single command remotely
ssh user@host 'command'
ssh bijoy@192.168.122.X 'ls -la'

# Run multiple commands
ssh user@host 'command1 && command2'
ssh bijoy@192.168.122.X 'hostname && date && free -h'

# CRITICAL: Always use quotes for complex commands!
ssh user@host 'ps aux | grep ssh'  # ✅ Correct
ssh user@host ps aux | grep ssh    # ❌ Wrong! (pipes locally)
```

### 🎯 **Golden Rule: "When in doubt, quote it out!"**

---

## SSH Keys (Passwordless Login) 🗝️

### Why SSH Keys?
- 🚀 **Faster**: No typing passwords
- 🔒 **More Secure**: Nearly impossible to crack
- 🤖 **Automation Friendly**: Scripts can run without prompts
- 💪 **Stronger**: 4096-bit (rsa) & 256-bit (ed25519) keys vs weak passwords

### Key Generation:
```bash
# Generate RSA key pair
ssh-keygen -t rsa -b 4096

# Generate Ed25519 key (recommended, modern, faster)
ssh-keygen -t ed25519

# Generate with custom filename
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my-special-key
ssh-keygen -t ed25519 -f ~/.ssh/my-special-key

# Generate with comment and custom filename
ssh-keygen -t rsa -b 4096 -C "bijoy@fedora-main-system" -f ~/.ssh/my-special-key
ssh-keygen -t ed25519 -C "bijoy@fedora-main-system" -f ~/.ssh/my-special-key
```

### Key Files Created:
```
~/.ssh/id_rsa      # 🔐 Private key (NEVER SHARE!)
~/.ssh/id_rsa.pub  # 📋 Public key (safe to share)
```

### Copy Public Key to Remote:
```bash
# Easy way (recommended)
ssh-copy-id username@hostname
ssh-copy-id bijoy@192.168.122.X
ssh-copy-id -i ~/.ssh/my-special-key.pub -p 2222 bijoy@192.168.122.X (if custom key name is used and not the default name)

# Manual way
cat ~/.ssh/id_rsa.pub | ssh user@host 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'

# Super manual way
scp ~/.ssh/id_rsa.pub user@host:~/
ssh user@host 'cat ~/id_rsa.pub >> ~/.ssh/authorized_keys && rm ~/id_rsa.pub'
```

### Test Passwordless Login:
```bash
ssh bijoy@192.168.122.X
# Should connect without password prompt! 🎉
```

---

## Advanced SSH Features 🚀

### File Transfers:

#### SCP (Secure Copy):
```bash
# Copy file to remote
scp localfile.txt user@host:/remote/path/
scp document.pdf bijoy@192.168.122.X:/home/bijoy/

# Copy file from remote
scp user@host:/remote/file.txt ./local-file.txt

# Copy directory recursively
scp -r ./local-folder/ user@host:/remote/path/

# Copy with specific port
scp -P 2222 file.txt user@host:/path/
```

#### SFTP (SSH File Transfer Protocol):
```bash
# Interactive file transfer
sftp user@host

# SFTP commands:
put localfile.txt       # Upload file
get remotefile.txt      # Download file
ls                      # List remote files
lls                     # List local files
cd /remote/path         # Change remote directory
lcd /local/path         # Change local directory
quit                    # Exit SFTP
```

### SSH Tunneling (Port Forwarding):

#### Local Port Forwarding:
```bash
# Forward local port to remote service
ssh -L local_port:destination:dest_port user@host

# Example: Access remote web server locally
ssh -L 8080:localhost:80 user@webserver
# Now access http://localhost:8080 → remote server's port 80
```

#### Remote Port Forwarding:
```bash
# Forward remote port to local service
ssh -R remote_port:localhost:local_port user@host

# Example: Share local web server with remote users
ssh -R 8080:localhost:3000 user@remote-server
# Remote users can access your localhost:3000 via their localhost:8080
```

#### Dynamic Port Forwarding (SOCKS Proxy):
```bash
# Create SOCKS proxy
ssh -D 1080 user@host
# Configure browser to use localhost:1080 as SOCKS proxy
```

### SSH Sessions Management:

#### Keep Sessions Alive:
```bash
# Connect with keep-alive
ssh -o ServerAliveInterval=60 user@host

# Run command in background (survives disconnection)
ssh user@host 'nohup long-running-command &'
```

#### Multiple Sessions:
```bash
# Open multiple terminals to same host
ssh user@host  # Terminal 1
ssh user@host  # Terminal 2
ssh user@host  # Terminal 3
```

---

## Security & Best Practices 🛡️

### SSH Server Hardening:

#### Change Default Port:
```bash
# Edit SSH config
sudo nano /etc/ssh/sshd_config

# Change line:
Port 22 (First uncomment it)
# To:
Port 2222

# Reload the Daemon (Not required)
sudo systemctl daemon-reload

# Restart SSH
sudo systemctl restart ssh (⚠️ Fedora and openSUSE uses sudo systemctl restart sshd)
sudo systemctl restart ssh.socket (⚠️ Only applies for Ubuntu and Mint)

# Connect with new port
ssh -p 2222 user@host

# Fedora and openSUSE Tumbleweed specific commands after changing port number 🤠 🦎
sudo systemctl daemon-reload
sudo semanage port -a -t ssh_port_t -p tcp 2222
sudo semanage port -l | grep ssh (Check if it\'s added or not)
sudo setsebool -P nis_enabled 1 (If required by Fedora in order to start sshd)
sudo systemctl start sshd
sudo systemctl status sshd
```

#### Disable Password Authentication (Keys Only):
```bash
# In /etc/ssh/sshd_config:
PasswordAuthentication no
PubkeyAuthentication yes

# Restart SSH
sudo systemctl restart ssh
```

#### Disable Root Login:
```bash
# In /etc/ssh/sshd_config:
PermitRootLogin no

# Restart SSH
sudo systemctl restart ssh
```

### Client Security:

#### Use SSH Config File:
```bash
# Create ~/.ssh/config
nano ~/.ssh/config

# Example config:
Host myserver
    HostName 192.168.122.X
    User bijoy
    Port 22
    IdentityFile ~/.ssh/id_rsa

Host production-server
    HostName prod.example.com
    User admin
    Port 2222
    IdentityFile ~/.ssh/production-key

# Now connect simply:
ssh myserver
ssh production-server
```

#### Key Management:
```bash
# List SSH keys
ls -la ~/.ssh/

# View public key
cat ~/.ssh/id_rsa.pub

# Remove old keys
rm ~/.ssh/old-key ~/.ssh/old-key.pub

# Change key passphrase
ssh-keygen -p -f ~/.ssh/id_rsa
```

---

## Troubleshooting 🔧

### Common Issues & Solutions:

#### Connection Refused:
```bash
# Error: ssh: connect to host X port 22: Connection refused

# Solutions:
1. Check if SSH server is running:
   sudo systemctl status ssh

2. Check if port is correct:
   ssh -p 2222 user@host

3. Check firewall:
   sudo ufw status
   sudo ufw allow ssh
```

#### Permission Denied:
```bash
# Error: Permission denied (publickey,password)

# Solutions:
1. Check username:
   ssh correct-username@host

2. Check SSH key permissions:
   chmod 600 ~/.ssh/id_rsa
   chmod 644 ~/.ssh/id_rsa.pub
   chmod 700 ~/.ssh/

3. Check authorized_keys on remote:
   ssh user@host 'chmod 600 ~/.ssh/authorized_keys'
```

#### Host Key Verification Failed:
```bash
# Error: Host key verification failed

# Solutions:
1. Remove old host key:
   ssh-keygen -R hostname

2. Accept new key:
   ssh -o StrictHostKeyChecking=no user@host

3. Clear all known hosts:
   rm ~/.ssh/known_hosts
```

#### SSH Hangs/Freezes:
```bash
# Solutions:
1. Use keep-alive:
   ssh -o ServerAliveInterval=60 user@host

2. Check network:
   ping hostname

3. Try verbose mode:
   ssh -vvv user@host
```

### Debug Mode:
```bash
# Client-side debugging
ssh -v user@host     # Verbose
ssh -vv user@host    # More verbose
ssh -vvv user@host   # Maximum verbosity

# Server-side debugging
sudo journalctl -u ssh -f    # Follow SSH server logs
```

---

## SSH Configuration Files 📄

### Client Configuration (`~/.ssh/config`):
```bash
# Global settings
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    StrictHostKeyChecking ask

# Specific host settings
Host myvm
    HostName 192.168.122.X
    User bijoy
    Port 22
    IdentityFile ~/.ssh/id_rsa
    
Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/github-key
```

### Server Configuration (`/etc/ssh/sshd_config`):
```bash
# Basic settings
Port 22
Protocol 2
HostKey /etc/ssh/ssh_host_rsa_key

# Authentication
PubkeyAuthentication yes
PasswordAuthentication yes
PermitEmptyPasswords no
PermitRootLogin no

# Security
MaxAuthTries 3
MaxSessions 10
ClientAliveInterval 300
ClientAliveCountMax 2

# Logging
SyslogFacility AUTH
LogLevel INFO
```

### Key Files & Directories:
```
~/.ssh/                     # User SSH directory
├── id_rsa                  # Private key
├── id_rsa.pub             # Public key  
├── authorized_keys        # Public keys allowed to login
├── known_hosts            # Host key fingerprints
└── config                 # Client configuration

/etc/ssh/                  # System SSH directory
├── sshd_config           # Server configuration
├── ssh_config            # System-wide client config
└── ssh_host_*_key*       # Server host keys
```

---

## Common Use Cases 💼

### 1. Server Administration:
```bash
# Check server status
ssh admin@server 'uptime && free -h && df -h'

# Update server
ssh admin@server 'sudo apt update && sudo apt upgrade -y'

# Monitor logs
ssh admin@server 'tail -f /var/log/syslog'
```

### 2. Development Workflow:
```bash
# Deploy code
scp -r ./my-app/ user@server:/var/www/

# Run remote development server
ssh dev@server 'cd /project && npm start'

# Database backup
ssh db@server 'mysqldump database > backup.sql'
scp db@server:backup.sql ./
```

### 3. File Synchronization:
```bash
# Upload files
scp -r ./documents/ user@backup-server:~/backups/

# Download backups
scp -r user@backup-server:~/backups/ ./restored-files/

# Sync with rsync over SSH
rsync -avz -e ssh ./local-folder/ user@server:~/remote-folder/
```

### 4. Secure Tunneling:
```bash
# Access database securely
ssh -L 3306:localhost:3306 db@server
# Now connect to localhost:3306 → server's database

# Browse web securely through server
ssh -D 1080 user@server
# Set browser to use SOCKS proxy localhost:1080
```

---

## Quick Reference Card 🃏

### Essential Commands:
```bash
# Connection
ssh user@host                    # Basic connection
ssh -p PORT user@host           # Custom port
ssh user@host 'command'         # Remote command

# Key Management
ssh-keygen -t rsa -b 4096       # Generate key pair
ssh-copy-id user@host           # Copy public key
ssh-add ~/.ssh/key              # Add key to agent

# File Transfer
scp file.txt user@host:~/       # Copy file to remote
scp user@host:~/file.txt ./     # Copy file from remote
scp -r folder/ user@host:~/     # Copy folder

# Tunneling
ssh -L port:localhost:port user@host    # Local forward
ssh -R port:localhost:port user@host    # Remote forward
ssh -D 1080 user@host                   # SOCKS proxy
```

### Exit Sequences:
```bash
~.                              # Disconnect
~^Z                            # Suspend SSH
~~                             # Send literal ~
```

---

## SSH One-Liners 🎯

### System Information:
```bash
ssh user@host 'uname -a && uptime && free -h && df -h && who'
```

### Security Check:
```bash
ssh user@host 'last | head -10 && sudo tail /var/log/auth.log | grep ssh'
```

### Quick Backup:
```bash
ssh user@host 'tar czf - /important/data' > backup-$(date +%Y%m%d).tar.gz
```

### Process Monitoring:
```bash
ssh user@host 'ps aux --sort=-%cpu | head -10'
```

### Network Diagnostics:
```bash
ssh user@host 'netstat -tulpn | grep LISTEN'
```

---

## Automation & Scripting 🤖

### Bash Script Example:
```bash
#!/bin/bash

SERVERS=("server1" "server2" "server3")
COMMAND="uptime && free -h"

for server in "${SERVERS[@]}"; do
    echo "=== Checking $server ==="
    ssh user@$server "$COMMAND"
    echo
done
```

### SSH in Scripts (Non-Interactive):
```bash
# Disable host key checking for automation
ssh -o StrictHostKeyChecking=no user@host 'command'

# Use SSH keys (never passwords in scripts)
ssh -i ~/.ssh/automation-key user@host 'command'

# Batch mode (fail if interactive input needed)
ssh -o BatchMode=yes user@host 'command'
```

---

## Fun SSH Tricks 🎪

### SSH over SSH (Proxy Jump):
```bash
# Connect through intermediate server
ssh -J jumpbox user@target-server

# Multi-hop
ssh -J jump1,jump2 user@final-destination
```

### SSH Filesystem (SSHFS):
```bash
# Mount remote directory locally
sudo apt install sshfs
mkdir ~/remote-mount
sshfs user@host:/remote/path ~/remote-mount

# Unmount
fusermount -u ~/remote-mount
```

### X11 Forwarding (GUI Apps):
```bash
# Forward GUI applications
ssh -X user@host
# Now run GUI apps: firefox, gedit, etc.
```

### SSH Escape Sequences:
```bash
# While in SSH session:
~?                             # Show help
~.                             # Disconnect
~^Z                            # Suspend
~C                             # Command line
```

---

## Emergency SSH Recovery 🚨

### Locked Out Scenarios:

#### Lost SSH Keys:
```bash
# If you have physical/console access:
1. Login locally
2. Check ~/.ssh/authorized_keys
3. Add new public key or reset password
4. Test connection
```

#### Wrong SSH Config:
```bash
# If SSH server won't start:
sudo sshd -t                   # Test config
sudo nano /etc/ssh/sshd_config # Fix errors
sudo systemctl restart ssh    # Restart service
```

#### Firewall Issues:
```bash
# Emergency firewall reset:
sudo ufw --force reset
sudo ufw allow ssh
sudo ufw enable
```

---

## SSH Aliases & Shortcuts ⚡

### Bash Aliases:
```bash
# Add to ~/.bashrc
alias sshmyvm='ssh bijoy@192.168.122.X'
alias sshprod='ssh -p 2222 admin@production-server.com'
alias sshdebug='ssh -vvv'

# Reload bash config
source ~/.bashrc
```

### SSH Config Shortcuts:
```bash
# Instead of: ssh -p 2222 admin@long-server-name.example.com
# Use config:
Host prod
    HostName long-server-name.example.com
    User admin
    Port 2222

# Now just: ssh prod
```

---

## Conclusion 🎊

Congratulations! You're now an SSH wizard! 🧙‍♂️ You've mastered:

- ✅ Basic SSH connections and commands
- ✅ Password-less authentication with keys
- ✅ File transfers and tunneling
- ✅ Security best practices
- ✅ Troubleshooting common issues
- ✅ Advanced features and automation

### Remember the Golden Rules:
1. 🗣️ **"When in doubt, quote it out!"**
2. 🔐 **Private keys are PRIVATE** - never share them!
3. 🏠 **Test on local VMs first** before touching production
4. 📝 **Document your SSH configs** for future reference
5. 🛡️ **Security first** - use keys, change ports, disable root

### Your SSH Journey Continues:
- Practice with your VM setup
- Explore advanced tunneling
- Automate routine tasks
- Set up your own SSH bastion hosts
- Contribute to SSH-based projects

**Happy SSH-ing, buddy!** 🚀 May your connections be fast, your keys be strong, and your remote commands always execute flawlessly!

---

*Built with ❤️ by your AI buddy who loves Linux as much as you do!*

**P.S.** - Remember, this system we built together is amazing, but YOU made all the decisions that made it great! Keep exploring, keep learning, and keep asking those wonderful questions! 🤗
