# Linux Learning Guides & Documentation

**Comprehensive Linux guides from beginner to advanced**

A curated collection of Linux documentation, tutorials, and reference materials covering system administration, command-line mastery, and Linux internals.

---

## 🎯 What This Includes

### Learning Materials
- **Linux Story**: Understanding Linux history and philosophy
- **Linux Guide**: Comprehensive beginner to intermediate guide
- **File Permissions**: Deep dive into Linux permission system
- **Hidden Gems**: Lesser-known Linux features and tricks

### Command References
- **Command Reference Manual**: Complete CLI command guide
- **Linux Commands Guide**: Practical command usage

### System Administration
- **File System**: From raw metal to working file system
- **Storage Love Story**: Understanding disk, partitions, formatting
- **PostgreSQL Permissions**: Database user management

### Distribution Guides
- **Arch Linux**: Installation and configuration
- **Void Linux**: Setup and package management
- **Slackware Linux**: Classic Linux distribution guide
- **Ubuntu**: Dual boot, installation, post-setup

### Advanced Topics
- **SSH**: Secure Shell configuration and usage
- **Virtual Machines**: QEMU/KVM setup and management
- **System Recovery**: Troubleshooting and repair

---

## 🚀 Quick Start

### For Beginners
Start with:
1. `Learning/0-The-Linux-Story.md` - Understand Linux philosophy
2. `Learning/1-Linux-Guide.md` - Core concepts
3. `Linux-Commands/1-The-Linux-Commands-Guide.md` - Essential commands

### For System Administrators
Focus on:
1. `File-System/` - Storage and file system management
2. `SSH/` - Secure remote access
3. `Virtual-Machine/` - VM setup and management

### For Distribution-Specific Needs
Navigate to `Distro-Guides/` and choose your distribution.

### Environment Setup (Optional)
Some guides reference database setups. If needed:
```bash
# Copy environment template
cp .env.example .env

# Edit with your passwords
nano .env

# Source when running database examples
source .env
```

---

## 📂 Repository Structure
```
linux-guides/
├── .env.example                             # Optional: for database examples
├── .env                                     # Optional: your passwords (not in git)
├── Learning/
│   ├── 0-The-Linux-Story.md                 # Linux history
│   ├── 1-Linux-Guide.md                     # Core guide
│   ├── 2-More-Linux-Guide.md                # Advanced topics
│   ├── 3-Linux-File-Permissions.md          # Permission system
│   ├── 4-Linux-Secrets-Hidden-Gems.md       # Tips & tricks
│   └── 5-Practice-Time.md                   # Hands-on exercises
├── Linux-Commands/
│   ├── 0-Linux-Command-Reference-Manual.md  # Command reference
│   └── 1-The-Linux-Commands-Guide.md        # Practical guide
├── File-System/
│   ├── 0-Raw-Metal-To-Working-File-System.md
│   └── 0-The-Epic-Storage-Love-Story.md
├── Distro-Guides/
│   ├── Arch-Linux-Guide/
│   ├── Slackware-Linux-Guide/
│   └── Void-Linux-Guide/
├── SSH/
│   └── 0-SSH-Reference-Manual.md
├── Virtual-Machine/
│   ├── 1-QEMU-KVM-Installation.md
│   ├── 2-Ubuntu-VM-Installation.md
│   ├── 3-Windows-VM-Installation.md
│   └── 4-Mounting-Windows-VM-Shared-Device.md
├── Ubuntu/
│   ├── Dual-Boot-System/
│   ├── Ubuntu-Installation/
│   ├── Ubuntu-Post-Installation/
│   └── Ubuntu-Updates/
├── Postgres-Permission/
│   └── 1-Postgres-Permission-Guide.md
└── Linux-Documentations/
    └── 0-Linux-Documentations-Guide.md
```

---

## 🎓 Learning Path

### Level 1: Foundation
1. Linux Story (philosophy)
2. Linux Guide (basics)
3. File Permissions (security fundamentals)
4. Linux Commands (practical skills)

### Level 2: Intermediate
1. File System (storage management)
2. SSH (remote access)
3. System documentation
4. Package management

### Level 3: Advanced
1. Virtual Machines
2. Distribution-specific guides
3. System recovery
4. Performance optimization

---

## 🛠️ Topics Covered

### Core Concepts
- Linux philosophy and history
- File system hierarchy
- User and permission management
- Process management
- Package management

### Command Line
- Essential commands
- Text processing
- File manipulation
- System monitoring
- Networking tools

### System Administration
- Storage management
- User administration
- Service management
- Security hardening
- Backup strategies

### Distributions
- Arch Linux (rolling release)
- Void Linux (runit init)
- Slackware (BSD-style)
- Ubuntu (Debian-based)

---

## 📝 Notes

- Guides tested on Fedora, Ubuntu, Arch, Void, and Slackware
- Commands shown for both systemd and non-systemd systems where applicable
- Focus on understanding concepts, not just memorizing commands
- `.env` file is optional - only needed if running database examples

---

## 🔒 Security

- `.env` file is in `.gitignore` (if you create it)
- Use `.env.example` as template for database examples
- Never commit passwords to git

---

## 🤝 Contributing

Educational content! Feel free to:
- Use these guides for learning
- Share with others
- Suggest corrections or improvements

---

## 📚 Recommended Reading

Books referenced in these guides:
- *The Linux Command Line* by William Shotts
- *How Linux Works* by Brian Ward
- *UNIX and Linux System Administration Handbook* by Evi Nemeth et al.

---

## ✨ Author

**Bijoy Nandi**
- GitHub: [@bijoynandi](https://github.com/bijoynandi)
- Learning: Linux for Data Engineering

---

*Last updated: March 2026*
