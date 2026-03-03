# 🗄️ Complete Database Setup Guide for Ubuntu 24.04

> A comprehensive, beginner-friendly guide to setting up SQLite, PostgreSQL, and DBeaver on Ubuntu 24.04

---

## 📋 Table of Contents

1. [🔧 SQLite Setup with DB Browser](#-sqlite-setup-with-db-browser)
2. [🐘 PostgreSQL Installation & Configuration](#-postgresql-installation--configuration)
3. [🎛️ pgAdmin4 Setup](#️-pgadmin4-setup)
4. [🦫 DBeaver Installation Guide](#-dbeaver-installation-guide)
5. [🔍 Troubleshooting](#-troubleshooting)
6. [📚 Quick Reference](#-quick-reference)

---

## 🔧 SQLite Setup with DB Browser

### What is DB Browser for SQLite?

DB Browser for SQLite is a **visual, open-source tool** that provides a user-friendly graphical interface for creating, designing, and editing SQLite database files. Perfect for beginners who want to work with databases without writing SQL commands.

### ⚡ Quick Installation

```bash
# Update package list
sudo apt update

# Install DB Browser for SQLite
sudo apt install sqlitebrowser
```

### 🚀 Launch Options

| Method | Command/Action |
|--------|----------------|
| **GUI** | Search "DB Browser" in Applications |
| **Terminal** | `sqlitebrowser` |

### ✅ What You Get

- **Graphical Interface**: Create and edit SQLite databases visually
- **No SQL Required**: Point-and-click database operations
- **File-based**: Perfect for small to medium projects
- **Cross-platform**: Works on Linux, Windows, and macOS

---

## 🐘 PostgreSQL Installation & Configuration

### What is PostgreSQL?

PostgreSQL is a **powerful, enterprise-grade** relational database system known for:
- **Reliability**: ACID-compliant transactions
- **Scalability**: Handles multiple users and complex operations
- **Feature-rich**: Advanced SQL features and extensions
- **Open Source**: Free with strong community support

### 📦 Installation Steps

```bash
# Install PostgreSQL server and additional utilities
sudo apt install postgresql postgresql-contrib

# Verify installation
sudo systemctl status postgresql
```

**What gets installed:**
- `postgresql`: Main database server
- `postgresql-contrib`: Additional utilities and extensions

### 👥 User Management

PostgreSQL uses its own user system. Let's set up both the default `postgres` user and create your personal account.

#### Step 1: Configure postgres User

```bash
# Switch to postgres system user
sudo -i -u postgres

# Access PostgreSQL prompt
psql

# Set password for postgres database user
\password postgres
# Enter your desired password twice

# Exit PostgreSQL prompt
\q

# Return to your regular user
exit
```

#### Step 2: Create Your Personal User

**Database Setup**

**First, source the environment variables:**
```bash
source /home/bijoy/Documents/Linux/.env
```

```bash
# Connect to PostgreSQL as postgres user
sudo -u postgres psql

# Create your user (replace 'bijoy' with your username)
CREATE USER bijoy WITH PASSWORD "$POSTGRES_PASSWORD";

# Grant database creation privileges
ALTER USER bijoy CREATEDB;

# Optional: Grant superuser privileges (use with caution)
ALTER USER bijoy WITH SUPERUSER;

# Exit PostgreSQL
\q
```

#### Step 3: Test Your New User

```bash
# Connect with your new user
psql -U bijoy -d postgres -h localhost
```

**Parameter explanation:**
- `-U bijoy`: Connect as user 'bijoy'
- `-d postgres`: Connect to the 'postgres' database
- `-h localhost`: Connect to the local server

### 🔄 Service Management

```bash
# Start PostgreSQL
sudo systemctl start postgresql

# Stop PostgreSQL
sudo systemctl stop postgresql

# Restart PostgreSQL
sudo systemctl restart postgresql

# Check status
sudo systemctl status postgresql
```

---

**Test some basic operations:**
```sql
psql -U postgres -h localhost  # Postgres root user login
sudo -u postgres psql  # Another way to root user login
psql -U bijoy -d postgres -h localhost  # Postres local user login with postgres database selected
sudo -u bijoy psql  # Postgres local user login with bijoy (custom database) database selected (you have to manually create this database before connecting with the local user name)
\du  # List all users
\l  # Show/ List All Databases (\list)
\c <database_name>  # Switch Databases or (\connect <database_name>)
\c or SELECT current_database();  # - Shows current database
\dt  # List all the tables inside a database
\d <table_name>  # Shows table structure (DESCRIBE table_name in MySQL)
\conninfo  # Shows the current database, user and host
\x  # Expanded view
\a  # Unalligned view
f ','  # Field separator is comma
q  # Quit the page
; or ctrl c  # Get back from postgres-# to normal view postgres=#

To create a new line for sql code simply press enter.
To execute code put semicolon at the end of line and press enter.
```

## 🎛️ pgAdmin4 Setup

### What is pgAdmin4?

pgAdmin4 is a **web-based administration platform** for PostgreSQL that provides:
- **Graphical Interface**: Visual database management
- **Query Editor**: Write and execute SQL queries
- **Database Designer**: Create tables, indexes, and relationships
- **Monitoring Tools**: Performance and activity monitoring

### 🔐 Why Special Installation is Needed

Unlike Windows/macOS bundles, Ubuntu requires separate installation of pgAdmin4 from the official repository since it's not in Ubuntu's default repositories.

### 📥 Installation Process

#### Step 1: Install Prerequisites

```bash
sudo apt install curl ca-certificates gnupg lsb-release
```

**Package purposes:**
- `curl`: Downloads files from the internet
- `ca-certificates`: Ensures secure connections
- `gnupg`: Handles encryption keys
- `lsb-release`: Provides Ubuntu version information

#### Step 2: Add pgAdmin4 GPG Key

```bash
curl -fsSL https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/packages-pgadmin-org.gpg
```

> **Security Note**: This cryptographic key ensures downloaded packages are authentic and untampered.

#### Step 3: Add pgAdmin Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/packages-pgadmin-org.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" | sudo tee /etc/apt/sources.list.d/pgladmin4.list
```

#### Step 4: Update and Install

```bash
# Update package list
sudo apt update

# Install pgAdmin4 desktop version
sudo apt install pgadmin4-desktop
```

### 🔗 Connecting pgAdmin to PostgreSQL

#### Initial Setup

1. **Launch pgAdmin4**: Find it in Applications menu or run `pgadmin4`
2. **Set Master Password**: This protects your saved connections (different from PostgreSQL passwords)

#### Register Server Connections

**For postgres user:**

1. Right-click **"Servers"** → **"Register"** → **"Server..."**
   
   > ⚠️ **Important**: Use "Register" → "Server..." NOT "Create" → "Server Group"

2. Fill connection details:

   **General Tab:**
   - **Name**: `PostgreSQL - postgres user`

   **Connection Tab:**
   - **Host**: `localhost`
   - **Port**: `5432`
   - **Database**: `postgres`
   - **Username**: `postgres`
   - **Password**: [your postgres password]

3. Click **"Save"**

**For your personal user:**

Repeat with these details:

**General Tab:**
- **Name**: `PostgreSQL - bijoy user`

**Connection Tab:**
- **Host**: `localhost`
- **Port**: `5432`
- **Database**: `postgres`
- **Username**: `bijoy`
- **Password**: `$POSTGRES_PASSWORD`

Click **"Save"**

### ✅ Verification

After successful connection, you should see both servers in the left panel with expandable nodes for:
- 📊 Databases
- 👥 Login/Group Roles
- 💾 Tablespaces
- ⚙️ Extensions

---

## 🦫 DBeaver Installation Guide

### What is DBeaver?

DBeaver is a **universal database tool** that supports multiple database types:
- **Multi-database**: MySQL, PostgreSQL, SQLite, Oracle, MongoDB, and more
- **User-friendly**: Intuitive graphical interface
- **Feature-rich**: SQL editor, data visualization, ER diagrams
- **Free**: Community edition available

### 🎯 Installation Methods

#### Method 1: APT Repository (Advanced)

**Benefits:**
- Automatic updates with system
- Official source integration
- Seamless dependency management

```bash
# Add DBeaver repository key
curl -fsSL https://dbeaver.io/debs/dbeaver.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/dbeaver.gpg

# Add repository
echo "deb [signed-by=/usr/share/keyrings/dbeaver.gpg] https://dbeaver.io/debs/dbeaver-ce /" | sudo tee /etc/apt/sources.list.d/dbeaver.list

# Update and install
sudo apt update
sudo apt install dbeaver-ce
```

#### Method 2: Snap Installation (Recommended)

**Why Snap?**
- ✅ Easiest installation
- ✅ Automatic updates
- ✅ Sandboxed security
- ✅ Works across Ubuntu versions

```bash
# Update system
sudo apt update

# Install snapd (if not present)
sudo apt install snapd

# Install DBeaver Community Edition
sudo snap install dbeaver-ce
```

**Launch options:**
- **Applications**: Search "DBeaver"
- **Terminal**: `dbeaver-ce`
- **Activities**: Super key + "DBeaver"

#### Method 3: .deb Package Installation

**When to use:**
- Snap not available
- Prefer traditional packages
- Need more installation control

```bash
# Download from https://dbeaver.io/download/
# Look for "Linux Debian package (installer)"

# Navigate to Downloads
cd ~/Downloads

# Install the package
sudo dpkg -i dbeaver-ce_*.deb

# Fix dependencies if needed
sudo apt install -f
```

### 🔧 Post-Installation Setup

#### Java Requirements

DBeaver requires Java Runtime Environment:

```bash
# Install Java if needed
sudo apt install default-jre

# For development features
sudo apt install default-jdk
```

#### First Launch Configuration

1. **Welcome Screen**: Skip tutorials or follow them (recommended for beginners)
2. **Workspace**: DBeaver creates a workspace directory for your projects
3. **Database Connections**: You'll need database credentials to connect

### 🔄 Updating DBeaver

| Installation Method | Update Command |
|-------------------|----------------|
| **Snap** | `sudo snap refresh dbeaver-ce` |
| **.deb** | Download new .deb and install |
| **APT** | `sudo apt update && sudo apt upgrade dbeaver-ce` |

### 🗑️ Uninstalling DBeaver

| Installation Method | Uninstall Command |
|-------------------|-------------------|
| **Snap** | `sudo snap remove dbeaver-ce` |
| **.deb** | `sudo apt remove dbeaver-ce` |
| **APT** | `sudo apt remove dbeaver-ce` |

**Complete removal (including config):**
```bash
sudo apt purge dbeaver-ce
rm -rf ~/.local/share/DBeaverData
```

---

## 🔍 Troubleshooting

### Common PostgreSQL Issues

#### ❌ "Connection to server failed"

**Possible causes and solutions:**

1. **PostgreSQL not running**
   ```bash
   sudo systemctl start postgresql
   ```

2. **Wrong password**
   ```bash
   sudo -u postgres psql
   \password postgres
   ```

3. **User doesn't exist**
   ```bash
   sudo -u postgres psql -c "\du"
   ```

#### ❌ "Package pgadmin4 is not available"

**Solution:** Use the official installation method with repository setup (not Ubuntu's default repos).

#### ❌ PostgreSQL won't start

**Check logs:**
```bash
sudo journalctl -u postgresql
```

### Common DBeaver Issues

#### ❌ "Command not found"

**Solutions:**
- **Snap**: Try `snap run dbeaver-ce`
- **Other**: Launch from Applications menu

#### ❌ Java-related errors

```bash
sudo apt install default-jdk
```

#### ❌ DBeaver unresponsive

```bash
# Close DBeaver completely
# Clear workspace
rm -rf ~/.local/share/DBeaverData/workspace6
# Restart DBeaver
```

### General Database Connection Issues

#### ❌ Permission denied

- Verify database credentials
- Check user permissions
- Ensure database service is running

#### ❌ Connection timeout

- Check firewall settings
- Verify host/port configuration
- Test network connectivity

---

## 📚 Quick Reference

### PostgreSQL Commands

#### Service Management
```bash
sudo systemctl start postgresql      # Start service
sudo systemctl stop postgresql       # Stop service
sudo systemctl restart postgresql    # Restart service
sudo systemctl status postgresql     # Check status
```

#### Database Connection
```bash
# Connect as postgres user
sudo -u postgres psql

# Connect as specific user
psql -U username -d database -h localhost
```

#### Inside PostgreSQL Prompt
```sql
\l          -- List databases
\du         -- List users
\dt         -- List tables
\q          -- Quit
\?          -- Help
```

### File System Locations

| Component | Location |
|-----------|----------|
| **PostgreSQL Config** | `/etc/postgresql/` |
| **pgAdmin4 Desktop** | `/usr/share/applications/pgadmin4.desktop` |
| **DBeaver Data** | `~/.local/share/DBeaverData/` |
| **Repository Keys** | `/usr/share/keyrings/` |
| **Sources Lists** | `/etc/apt/sources.list.d/` |

### Port Numbers

| Service | Default Port |
|---------|-------------|
| **PostgreSQL** | 5432 |
| **pgAdmin4 Web** | 80/443 |
| **MySQL** | 3306 |
| **MongoDB** | 27017 |

### Essential Ubuntu Commands

```bash
# Package management
sudo apt update                 # Update package lists
sudo apt upgrade               # Upgrade installed packages
sudo apt install package      # Install package
sudo apt remove package       # Remove package
sudo apt purge package        # Remove package + config

# System information
lsb_release -a                # Ubuntu version
uname -a                      # System information
whoami                        # Current user
```

---

## 🎯 Summary

You now have a complete database environment on Ubuntu 24.04:

### 🔧 What's Installed

- **SQLite**: DB Browser for lightweight, file-based databases
- **PostgreSQL**: Enterprise-grade database server on port 5432
- **pgAdmin4**: Web-based PostgreSQL management interface
- **DBeaver**: Universal database client for multiple database types

### 👥 User Accounts

- **postgres**: PostgreSQL superuser for administrative tasks
- **bijoy**: Personal PostgreSQL user with database creation privileges

### 🔗 Connections Configured

- Both users set up in pgAdmin4 with proper connection parameters
- DBeaver ready to connect to any supported database

### 🎨 Flexibility

This setup provides the flexibility to work with:
- **Simple projects**: SQLite with DB Browser
- **Complex applications**: PostgreSQL with pgAdmin4 or DBeaver
- **Multi-database environments**: DBeaver for everything

---

## 🚀 Next Steps

1. **Practice with Sample Data**: Create test databases and tables
2. **Learn SQL**: Use the query editors in pgAdmin4 or DBeaver
3. **Explore Features**: Discover data visualization and ER diagram tools
4. **Backup Strategies**: Learn about database backup and recovery
5. **Security**: Implement proper user permissions and access controls

---

**Happy databasing! 🎉**

> **Note**: This guide was created for Ubuntu 24.04. Commands may vary slightly for other Ubuntu versions.

---

*Last updated: July 2025*
