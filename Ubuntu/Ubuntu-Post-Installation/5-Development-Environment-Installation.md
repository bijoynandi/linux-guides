# 🚀 Complete Ubuntu Development Environment Setup Guide

> **Your comprehensive roadmap to setting up a professional Python development environment on Ubuntu**

## 📋 Table of Contents

1. [🐍 Anaconda Installation Guide](#-anaconda-installation-guide)
2. [💻 VS Code Installation Guide](#-vs-code-installation-guide)
3. [🧠 PyCharm Community Installation Guide](#-pycharm-community-installation-guide)
4. [🔧 Essential Commands Reference](#-essential-commands-reference)
5. [🆘 Troubleshooting Guide](#-troubleshooting-guide)
6. [📚 Additional Resources](#-additional-resources)

---

# 🐍 Anaconda Installation Guide

## 🎯 What is Anaconda and Why Do We Need It?

**Anaconda** is a Python distribution that comes with everything you need for data science and development:

| Feature | Description |
|---------|-------------|
| 🐍 **Python Interpreter** | The program that runs Python code |
| 📦 **Conda Package Manager** | Installs Python libraries easily |
| 🔢 **500+ Pre-installed Packages** | numpy, pandas, matplotlib, and more |
| 🏠 **Environment Management** | Keep different projects separate |
| 📓 **Jupyter Notebooks** | Interactive coding environment |

### 🆚 Why Anaconda vs Regular Python?

| Aspect | Regular Python | Anaconda |
|--------|---------------|----------|
| **Safety** | ❌ Can interfere with system | ✅ Isolated from system Python |
| **Dependencies** | ❌ Complex conflicts | ✅ Automatic resolution |
| **Completeness** | ❌ Need manual setup | ✅ Everything included |
| **Environments** | ❌ Manual management | ✅ Built-in isolation |

## 🛠️ Prerequisites

Before we start, ensure you have:

- ✅ Ubuntu system (any recent version)
- ✅ Internet connection
- ✅ About 3GB free disk space
- ✅ Terminal access

### 🧠 Understanding Key Concepts

| Term | Definition |
|------|------------|
| **Terminal** | The command-line interface - your direct line to the computer |
| **PATH** | List of directories where Ubuntu looks for programs |
| **Environment Variables** | Special variables that programs use (PATH is one) |

## 📋 Step-by-Step Installation Process

### Step 1: Update Your System

```bash
sudo apt update && sudo apt upgrade -y
```

> **🔍 Command Breakdown:**
> - `sudo` = Run as administrator
> - `apt` = Ubuntu's package manager
> - `update` = Download latest package list
> - `upgrade` = Install available updates
> - `-y` = Automatically answer "yes" to prompts

**💡 Why this matters:** Ensures latest security patches and prevents conflicts

### Step 2: Install Required Tools

```bash
sudo apt install curl wget -y
```

> **🔍 What these do:**
> - `curl` & `wget` = Tools to download files from the internet
> - More reliable than browser downloads for large files

### Step 3: Download Anaconda Installer

```bash
cd ~
wget https://repo.anaconda.com/archive/Anaconda3-2025.06-1-Linux-x86_64.sh
```

> **🔍 Command Breakdown:**
> - `cd ~` = Go to home directory
> - `wget` = Download the file
> - The filename tells us: Anaconda3 (Python 3), version, Linux 64-bit

### Step 4: Verify Download (Optional but Recommended)

```bash
    ls -la Anaconda3-2025.06-1-Linux-x86_64.sh
```

> **✅ What to expect:** File size around 900MB-1GB

### Step 5: Make Installer Executable

```bash
chmod +x Anaconda3-2025.06-1-Linux-x86_64.sh
```

> **🔍 Security Note:** Downloaded files don't have execute permission by default

### Step 6: Run the Installer

```bash
bash Anaconda3-2025.06-1-Linux-x86_64.sh
```

### Step 7: Navigate Installation Prompts

| Prompt | Action | Why |
|--------|--------|-----|
| **Welcome Message** | Press `Enter` | Continue to next step |
| **License Agreement** | Type `yes` | Accept terms |
| **Installation Location** | Press `Enter` | Accept default (`/home/username/anaconda3`) |
| **🚨 conda init Question** | Type `yes` | **CRITICAL**: Makes conda available in terminal |

> **⚠️ Important:** Many beginners miss the conda init step - always type `yes`!

### Step 8: Activate Installation

```bash
source ~/.bashrc
```

> **💡 Alternative:** Close and reopen terminal (same effect)

### Step 9: Verify Installation

```bash
conda --version
conda list
```

> **✅ Expected output:**
> - Version number (e.g., `conda 24.5.0`)
> - Long list of installed packages

### Step 10: Update Conda (Never run on base environment atleast ⚠️)

```bash
conda update conda ❌
```

### Step 11: Test Your Installation

```bash
conda create --name test-env python=3.11
conda activate test-env
python --version
conda deactivate
```

> **✅ What you should see:**
> - Environment creation progress
> - Prompt changes to `(test-env)` when active
> - Python version displayed

### Step 12: Clean Up

```bash
rm ~/Anaconda3-2025.06-1-Linux-x86_64.sh
```

## 🎉 What Just Happened?

### Your PATH Changed

**Before:** `/usr/bin:/bin:/usr/local/bin`
**After:** `/home/username/anaconda3/bin:/home/username/anaconda3/condabin:/usr/bin:/bin`

This means when you type `python`, Ubuntu finds Anaconda's Python first!

### Your Terminal Prompt Changed

**Before:** `username@computer:~$`
**After:** `(base) username@computer:~$`

The `(base)` shows you're in Anaconda's base environment.

### What's Now Installed

Anaconda includes 500+ packages:

| Category | Key Packages |
|----------|--------------|
| **Math & Arrays** | numpy, scipy |
| **Data Analysis** | pandas, matplotlib |
| **Machine Learning** | scikit-learn, tensorflow |
| **Interactive Computing** | jupyter, ipython |
| **Web Development** | flask, requests |

## 🔧 Essential Commands Reference

### Environment Management
```bash
# Create new environment
conda create --name myproject python=3.11

# Activate environment
conda activate myproject

# Deactivate environment
conda deactivate

# List environments
conda env list

# Remove environment
conda env remove --name myproject
```

### Package Management
```bash
# Install package
conda install pandas numpy matplotlib

# List installed packages
conda list

# Update all packages
conda update --all ❌

# Search for packages
conda search packagename

# Install from specific channel
conda install -c conda-forge packagename
```

## Setting up anaconda with custom environment (data_analytics)

conda create -n ENVNAME python=3.12 PACKAGES

```bash
conda create -n data_analytics python=3.12
```

conda activate ENVNAME

```bash
conda activate data_analytics
```

```bash
conda install -c conda-forge numpy pandas matplotlib seaborn plotly scikit-learn datasets jupyter ipykernel ipywidgets jupysql psycopg2 openpyxl django
```

## 🩺 Diagnosing Anaconda

```bash
conda doctor
conda doctor -n env_name
```

```bash
conda clean -all
```

## ❓ Common Questions & Solutions

### Q: Can I remove the `(base)` from my prompt?
```bash
conda config --set auto_activate_base false
```

### Q: How do I know which Python I'm using?
```bash
which python
# Should show: /home/username/anaconda3/bin/python
```

### Q: How to completely remove Anaconda?
```bash
# Remove directory
rm -rf ~/anaconda3

# Remove conda init from ~/.bashrc
# (Edit ~/.bashrc and remove the conda initialize section)
```

---

# 💻 VS Code Installation Guide

## 🎯 What is VS Code?

**Visual Studio Code** is a free, lightweight, yet powerful code editor that's become the go-to choice for millions of developers worldwide.

### 🌟 Why VS Code?

| Feature | Benefit |
|---------|---------|
| **Cross-platform** | Works on Linux, Windows, macOS |
| **Rich Ecosystem** | Thousands of extensions |
| **Built-in Features** | Terminal, Git, debugging |
| **Regular Updates** | Active Microsoft support |

## 📋 Installation Methods

### 🥇 Method 1: Snap Installation (Recommended for Beginners)

#### What is Snap?
Snap is Ubuntu's app store system - think of it as installing from a secure, sandboxed environment.

```bash
# Update snap
sudo snap refresh

# Install VS Code
sudo snap install code --classic

# Launch VS Code
code
```

> **🔍 Command Breakdown:**
> - `--classic` = Allows VS Code to access your entire system

### 🥈 Method 2: Download Microsoft's GPG key (the right way)

#### Step 1: Add Microsoft's GPG Key
```bash
sudo apt install curl

curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /usr/share/keyrings/microsoft.gpg
```

#### Step 2: Add repository (modern .sources format)
```bash
echo "Types: deb
URIs: https://packages.microsoft.com/repos/code
Suites: stable
Components: main
Architectures: amd64 arm64 armhf
Signed-By: /usr/share/keyrings/microsoft.gpg" | sudo tee /etc/apt/sources.list.d/vscode.sources
```

#### Step 3: Install
```bash
sudo apt update
sudo apt install code
```

#### Step 4: Launch code
```bash
code
```

### 🥉 Method 3: .deb Package

1. Download from [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Install the downloaded .deb file:
```bash
cd ~/Downloads
sudo dpkg -i code_*.deb
sudo apt install -f  # Fix dependencies if needed
```

## 🎨 Essential First-Time Setup

### Must-Have Extensions

| Extension | Purpose |
|-----------|---------|
| **Python** | Python development support |
| **GitLens** | Enhanced Git capabilities |
| **Black Formatter** | Python Code formatting |
| **Auto Rename Tag** | HTML/XML tag renaming |
| **Bracket Pair Colorizer** | Visual bracket matching |

### Key Settings
- **Auto Save**: File → Auto Save
- **Font Size**: Ctrl + , → Search "font size"
- **Theme**: Ctrl + K, Ctrl + T

### Integrated Terminal
- **Open**: Ctrl + ` (backtick)
- **New Terminal**: Ctrl + Shift + `

## 🚀 Quick Reference

| Action | Shortcut |
|--------|----------|
| **Open VS Code** | `code` |
| **Open current directory** | `code .` |
| **Open file** | `code filename.txt` |
| **Settings** | `Ctrl + ,` |
| **Command Palette** | `Ctrl + Shift + P` |
| **Terminal** | `Ctrl + `` |
| **Check version** | `code --version` |

---

# 🧠 PyCharm Community Installation Guide

## 🎯 What is PyCharm Community?

**PyCharm Community** is a free, open-source IDE specifically designed for Python development. It's like a specialized workshop for Python programmers.

### 📊 System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **RAM** | 2GB | 4GB+ |
| **Storage** | 2.5GB | 5GB+ |
| **OS** | Ubuntu 18.04+ | Ubuntu 20.04+ |

## 🏆 Method 1: Manual Installation (Recommended)

### Step 1: Prepare System

```bash
# Update system
sudo apt update

# Install Java (required for PyCharm)
sudo apt install openjdk-17-jdk

# Navigate to Downloads
cd ~/Downloads
```

### Step 2: Download PyCharm

#### Option A: Latest Stable Version
```bash
wget https://download.jetbrains.com/python/pycharm-2025.2.1.1.tar.gz
```

#### Option B: Auto-detect Latest (Advanced)
```bash
LATEST_URL=$(curl -s "https://data.services.jetbrains.com/products/releases?code=PCC&latest=true&type=release" | grep -o '"downloadUrl":"[^"]*' | cut -d'"' -f4)
echo "Downloading: $LATEST_URL"
wget $LATEST_URL
```

### Step 3: Extract and Install

```bash
# Extract archive
tar -xzf pycharm-*.tar.gz

# Check extracted folder
ls -la pycharm-*

# Clean up
rm ~/Downloads/pycharm-*.tar.gz

# Move to system directory
sudo mv pycharm-* /opt/

# Create symbolic link (IMPORTANT: Use native launcher!)
sudo ln -s /opt/pycharm-*/bin/pycharm /usr/local/bin/pycharm
```

> **🚨 Critical Note:** Always use the native launcher (`pycharm`) NOT the script launcher (`pycharm.sh`) to avoid annoying notifications!

### Step 4: Create Desktop Entry

```bash
sudo nano /usr/share/applications/pycharm.desktop
```

**Copy this content:** (Always use absolute paths, Desktop files need absolute)
```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=PyCharm
Comment=Python IDE for Professional Developers
Exec=/opt/pycharm-2025.2.1.1/bin/pycharm
Icon=/opt/pycharm-2025.2.1.1/bin/pycharm.png
Path=/opt/pycharm-2025.2.1.1/bin/
StartupNotify=true
StartupWMClass=jetbrains-pycharm
Categories=Development;IDE;
```

> **💾 Save and exit:** `Ctrl + O`, `Enter`, `Ctrl + X`

### Step 5: Finalize Installation

```bash
# Make desktop entry executable
sudo chmod +x /usr/share/applications/pycharm.desktop

# Update applications menu
sudo update-desktop-database

# Launch
pycharm
```

## 🚀 Method 2: Snap Installation (Simpler)

```bash
# Install snapd if needed
sudo apt install snapd

# Install PyCharm Community
sudo snap install pycharm-community --classic

# Launch
pycharm-community
```

## 🔄 Updating PyCharm

```bash
# Download latest version
cd ~/Downloads
wget [latest-pycharm-url]

# Remove old version
sudo rm -rf /opt/pycharm-*

# Extract and install new version
tar -xzf pycharm-*.tar.gz
sudo mv pycharm-* /opt/

# Update symbolic link
sudo rm /usr/local/bin/pycharm
sudo ln -s /opt/pycharm-*/bin/pycharm /usr/local/bin/pycharm

# Update desktop entry
sudo update-desktop-database
```

## 🗑️ Complete Uninstallation

```bash
# Remove installation
sudo rm -rf /opt/pycharm-*

# Remove symbolic link
sudo rm /usr/local/bin/pycharm

# Remove desktop entry
sudo rm /usr/share/applications/pycharm.desktop

# Remove user configurations
rm -rf ~/.config/JetBrains/PyCharm*
rm -rf ~/.cache/JetBrains/PyCharm*
rm -rf ~/.local/share/JetBrains/PyCharm*

# Update applications menu
sudo update-desktop-database

# Follow the pycharm installation like disabling and installing profile
The IDE is running with enabled input methods that can cause freezes. Please consider to disable input methods if you do not really use them.

Disable input method.

The system restricts the embedded browser from running with the sandbox enabled. A corresponding AppArmor profile must be installed to start the browser sandboxed.

Install profile... Disable sandbox Learn more...
```

---

# 🔧 Essential Commands Reference

## 📂 File and Directory Operations

| Command | Purpose | Example |
|---------|---------|---------|
| `ls` | List files | `ls -la` (detailed list) |
| `cd` | Change directory | `cd ~/Downloads` |
| `mkdir` | Create directory | `mkdir my_project` |
| `rm` | Remove files | `rm filename.txt` |
| `rm -rf` | Remove directory | `rm -rf directory/` |
| `cp` | Copy files | `cp file1.txt file2.txt` |
| `mv` | Move/rename | `mv oldname.txt newname.txt` |
| `chmod` | Change permissions | `chmod +x script.sh` |
| `pwd` | Show current directory | `pwd` |

## 📦 Package Management

### APT Commands
| Command | Purpose |
|---------|---------|
| `sudo apt update` | Update package lists |
| `sudo apt upgrade` | Upgrade installed packages |
| `sudo apt install package` | Install package |
| `sudo apt remove package` | Remove package |
| `sudo apt search package` | Search for package |

### Snap Commands
| Command | Purpose |
|---------|---------|
| `sudo snap install package` | Install snap package |
| `sudo snap remove package` | Remove snap package |
| `sudo snap refresh` | Update all snaps |
| `snap list` | List installed snaps |

## 🔍 System Information

| Command | Purpose |
|---------|---------|
| `uname -a` | System information |
| `lsb_release -a` | Ubuntu version |
| `df -h` | Disk usage |
| `free -h` | Memory usage |
| `ps aux` | Running processes |
| `top` | System monitor |

## 🌐 Network and Downloads

| Command | Purpose |
|---------|---------|
| `wget URL` | Download file |
| `curl URL` | Transfer data |
| `ping google.com` | Test connectivity |
| `ifconfig` | Network configuration |

---

# 🆘 Troubleshooting Guide

## 🐍 Anaconda Issues

### Problem: `conda: command not found`
**Solution:**
```bash
# Check if conda init ran
tail -20 ~/.bashrc

# If missing, run conda init
~/anaconda3/bin/conda init
source ~/.bashrc
```

### Problem: Environment activation fails
**Solution:**
```bash
# Restart terminal or
source ~/.bashrc

# Or manually activate
source ~/anaconda3/bin/activate
```

## 💻 VS Code Issues

### Problem: `code: command not found`
**Solution:**
```bash
# If installed via snap
sudo snap install code --classic

# If installed via apt
sudo apt install code
```

### Problem: Extensions not working
**Solution:**
```bash
# Start without extensions
code --disable-extensions

# Reset extensions
rm -rf ~/.vscode/extensions
```

## 🧠 PyCharm Issues

### Problem: PyCharm won't start
**Solution:**
```bash
# Check Java installation
java -version

# Install Java if missing
sudo apt install openjdk-17-jdk

# Try launching from terminal
pycharm
```

### Problem: "Switch to native launcher" notification
**Solution:**
```bash
# Remove old symbolic link
sudo rm /usr/local/bin/pycharm

# Create new link to native launcher
sudo ln -s /opt/pycharm-*/bin/pycharm /usr/local/bin/pycharm
```

## 🔧 General Linux Issues

### Problem: Permission denied
**Solution:**
```bash
# Check file permissions
ls -la filename

# Add execute permission
chmod +x filename

# Change ownership
sudo chown $USER:$USER filename
```

### Problem: Disk space issues
**Solution:**
```bash
# Check disk usage
df -h

# Find large files
du -sh * | sort -rh | head -10

# Clean package cache
sudo apt autoclean
sudo apt autoremove
```

---

# 📚 Additional Resources

## 🔗 Official Documentation

- **Anaconda:** [docs.anaconda.com](https://docs.anaconda.com)
- **VS Code:** [code.visualstudio.com/docs](https://code.visualstudio.com/docs)
- **PyCharm:** [jetbrains.com/pycharm/documentation](https://www.jetbrains.com/pycharm/documentation/)

## 🎓 Learning Resources

### Python Learning
- **Python.org Tutorial:** [python.org/tutorial](https://docs.python.org/3/tutorial/)
- **Real Python:** [realpython.com](https://realpython.com)
- **Automate the Boring Stuff:** [automatetheboringstuff.com](https://automatetheboringstuff.com)

### Linux Command Line
- **Linux Command Line Basics:** [linuxcommand.org](http://linuxcommand.org)
- **Ubuntu Documentation:** [help.ubuntu.com](https://help.ubuntu.com)

## 🚀 Next Steps

### For Beginners
1. **Create your first Python project** in PyCharm
2. **Learn basic terminal commands** from the reference section
3. **Install additional packages** using conda/pip
4. **Set up version control** with Git

### For Intermediate Users
1. **Configure development workflows** with VS Code
2. **Create custom conda environments** for different projects
3. **Explore advanced PyCharm features** like debugging and testing
4. **Learn Docker** for containerized development

### For Advanced Users
1. **Set up CI/CD pipelines** with GitHub Actions
2. **Configure remote development** environments
3. **Explore Jupyter Lab** for data science workflows
4. **Implement code quality tools** like linters and formatters

---

## 💡 Pro Tips

### 🎯 Environment Management
- **Always use separate environments** for different projects
- **Never install packages in the base environment**
- **Document your environments** with `conda env export > environment.yml`
- **Use meaningful environment names** like `web-scraping` or `data-analysis`

### 🔧 Development Workflow
- **Use the integrated terminal** in your IDE
- **Learn keyboard shortcuts** for faster development
- **Set up code formatting** tools like Black or Prettier
- **Use version control** from day one

### 📊 Performance Tips
- **Close unused applications** to free up RAM
- **Use SSD storage** for faster file operations
- **Regular system updates** for security and performance
- **Monitor resource usage** with `htop` or system monitor

---

## 🎉 Congratulations!

You've successfully set up a complete Python development environment on Ubuntu! Your system now includes:

- ✅ **Anaconda** for package and environment management
- ✅ **VS Code** for lightweight code editing
- ✅ **PyCharm Community** for comprehensive Python development
- ✅ **Essential command-line tools** for system management

**Remember:** The terminal is your friend! While it might seem intimidating at first, it's actually more reliable and predictable than GUI tools. Every command does exactly what it says, and you can always use `command --help` for assistance.

---

*Happy coding! 🚀*

> **💬 Need help?** Don't hesitate to consult the troubleshooting section or search for specific error messages online. The Linux community is incredibly helpful and welcoming to newcomers!
