# 🐳 Complete Docker Installation and Usage Guide for Ubuntu

> **Your comprehensive journey from Docker zero to hero!**
> This guide will take you step-by-step through Docker installation, SQL Server setup, and advanced management tools.

---

## 📋 Table of Contents

1. [🤔 What is Docker?](#what-is-docker)
2. [✅ Prerequisites](#prerequisites)
3. [🚀 Installing Docker](#installing-docker)
4. [⚙️ Post-Installation Setup](#post-installation-setup)
5. [🧹 System Cleanup and Maintenance](#system-cleanup-and-maintenance)
6. [📚 Docker Basics](#docker-basics)
7. [🗄️ Setting up SQL Server in Docker](#setting-up-sql-server-in-docker)
8. [🔄 Restoring SQL Server Backup Files](#restoring-sql-server-backup-files)
9. [🎛️ Installing Portainer for Docker GUI Management](#installing-portainer-for-docker-gui-management)
10. [⚡ Quick Reference Commands](#quick-reference-commands)
11. [🔧 Essential Docker Commands](#essential-docker-commands)
12. [🚨 Troubleshooting](#troubleshooting)
13. [💡 Best Practices](#best-practices)
14. [🎯 Next Steps](#next-steps)

---

## What is Docker?

Docker is like a **"shipping container"** for software applications! Just as shipping containers revolutionized global trade by standardizing how goods are transported, Docker containers revolutionize software deployment by standardizing how applications are packaged and run.

### 🧠 Key Concepts

| Concept | Description | Real-world Analogy |
|---------|-------------|-------------------|
| **Container** | A lightweight, portable package with everything needed to run an application | A shipping container with all your stuff inside |
| **Image** | A blueprint/template used to create containers | A blueprint for building a house |
| **Dockerfile** | Text file with instructions to build an image | A recipe for cooking |
| **Docker Hub** | Online repository of pre-built images | An app store for containers |

### 🎯 Why Use Docker?

> **"It works on my machine"** becomes **"It works everywhere"**

- **🔄 Consistency**: Same environment everywhere
- **🏠 Isolation**: Applications don't interfere with each other
- **📦 Portability**: Easy to move between development, testing, and production
- **⚡ Efficiency**: Lighter than virtual machines
- **🚀 Scalability**: Easy to scale up or down

---

## Prerequisites

Before we dive into Docker, let's ensure your system is ready!

### 🖥️ System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **OS** | Ubuntu 20.04 LTS (64-bit) | Ubuntu 22.04 LTS or newer |
| **RAM** | 4GB | 8GB+ |
| **Storage** | 20GB free | 50GB+ free |
| **CPU** | 2 cores | 4+ cores |

### 🔍 Check Your Ubuntu Version

```bash
lsb_release -a
```

**Expected output:**
```
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.2 LTS
Release:        24.04
Codename:       noble
```

### 🔄 Update Your System

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

> **💡 What this does:** Updates your package list and upgrades all installed packages to their latest versions. This ensures compatibility and security.

---

## Installing Docker

We'll install Docker using the official Docker repository for the most up-to-date version.

### 🧹 Step 1: Uninstall old versions (If any)

Before you can install Docker Engine, you need to uninstall any conflicting packages.

Your Linux distribution may provide unofficial Docker packages, which may conflict with the official packages provided by Docker. You must uninstall these packages before you install the official version of Docker Engine.

The unofficial packages to uninstall are:

docker.io
docker-compose
docker-compose-v2
docker-doc
podman-docker

Moreover, Docker Engine depends on containerd and runc. Docker Engine bundles these dependencies as one bundle: containerd.io. If you have installed the containerd or runc previously, uninstall them to avoid conflicts with the versions bundled with Docker Engine.

Run the following command to uninstall all conflicting packages:
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

> **💡 Why these steps?** Removes any conflicting old Docker installations that might interfere with the new installation.


### 📦 Step 2: Install Prerequisites

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
```

**What each package does:**

| Package | Purpose |
|---------|---------|
| `ca-certificates` | Contains trusted certificate authorities |
| `curl` | Tool for downloading files from the internet |

```bash
sudo install -m 0755 -d /etc/apt/keyrings # Creates a directory with specific permissions.
```
<!-- sudo = run as root/administrator
install = command that can copy files OR create directories
-m 0755 = set permissions to 0755 (owner can read/write/execute, group and others can read/execute)
-d = create a directory (not copy a file)
/etc/apt/keyrings = the directory path to create -->

### 🔐 Step 3: Add Docker's Official GPG Key

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
<!-- curl = download tool
-f = fail silently on HTTP errors
-s = silent mode (no progress bar)
-S = show errors even in silent mode
-L = follow redirects if the URL has moved
https://download.docker.com/linux/ubuntu/gpg = the URL to download from
-o /etc/apt/keyrings/docker.asc = save the downloaded file to this location 
chmod = change mode (permissions)
a+r = all users + read permission
/etc/apt/keyrings/docker.asc = Docker's GPG public key file -->

> **🔒 Security Note:** This adds Docker's official GPG key to verify that packages come from Docker and haven't been tampered with.

### 📂 Step 4: Add Docker Repository

```bash
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
<!-- deb = tells apt this is a Debian package repository
[arch=$(dpkg --print-architecture)] = gets your CPU architecture (like amd64, arm64)
signed-by=/etc/apt/keyrings/docker.asc = use that GPG key we just downloaded to verify packages
https://download.docker.com/linux/ubuntu = Docker's repository URL
$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") = gets your Ubuntu version codename (like "jammy", "focal", "noble")
stable = use the stable release channel -->

<!-- The pipeline part:
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

| = pipe the echo output to the next command
sudo tee /etc/apt/sources.list.d/docker.list = write that repository info to a new file
> /dev/null = hide the output from tee (it normally shows what it writes) 

What it creates:
A file at /etc/apt/sources.list.d/docker.list containing something like:

deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu jammy stable-->


> **📍 What this does:** Adds Docker's official repository to your system's package sources, so you can install Docker using `apt`.

### 🔄 Step 5: Update Package Index

```bash
sudo apt-get update
```
<!-- sudo apt-get update:
Refreshes the package lists, including the new Docker repository we just added.

Now apt knows where to find Docker packages and can verify they're authentic! -->


### 🐳 Step 6: Install Docker Engine

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Component breakdown:**

| Component | Purpose |
|-----------|---------|
| `docker-ce` | The main Docker engine |
| `docker-ce-cli` | Command-line interface for Docker |
| `containerd.io` | Container runtime |
| `docker-buildx-plugin` | Enhanced build capabilities |
| `docker-compose-plugin` | Multi-container application management |

### ✅ Step 7: Verify Installation

```bash
sudo docker run hello-world
```

**Expected output:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

> **🎉 Congratulations!** Docker is now installed and working!

---

## Post-Installation Setup

### 👥 Add Your User to the Docker Group

By default, Docker requires `sudo` for every command. Let's fix that:

```bash
sudo usermod -aG docker $USER
```

> **💡 What this does:** Adds your current user to the `docker` group, allowing you to run Docker commands without `sudo`.

### 🔄 Restart Your Computer

```bash
sudo reboot
```

> **⚠️ CRITICAL:** You **MUST** restart your computer for the group membership change to take effect. Don't skip this step!

### ✅ Verify Group Membership (After Restart)

```bash
groups $USER
```

You should see `docker` listed among your groups.
bijoy : bijoy adm cdrom sudo dip plugdev users lpadmin docker

### 🧪 Test Without Sudo (After Restart)

```bash
docker run hello-world
```

If this works without `sudo`, you're all set! 🎉

### 🚀 Configure Docker to Start on Boot

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

> **💡 What this does:** Ensures Docker starts automatically when your system boots.

---

## System Cleanup and Maintenance

### 🗑️ Remove Unnecessary Installation Files

After Docker installation, clean up temporary files:

```bash
# Clean apt cache to free up space
sudo apt autoclean
sudo apt autoremove

# Remove downloaded package files
sudo apt clean
```

**Benefits:**
- ✅ Frees up disk space
- ✅ Removes orphaned packages
- ✅ Cleans cached files

---

## 🧹 Uninstall Docker Engine

Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes:

```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

Remove source list and keyrings
```bash
sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.asc
```
You have to delete any edited configuration files manually.

---

## Docker Basics

### 🖼️ Understanding Images vs Containers

| **Docker Image** | **Docker Container** |
|------------------|---------------------|
| 📋 Template/Blueprint | 🏃 Running Instance |
| 📖 Read-only | ✏️ Writable |
| 📤 Can be shared | 🔒 Isolated |
| 💾 Stored on disk | 🧠 Active in memory |

**Think of it like this:**
- **Image** = Recipe for a cake 📋
- **Container** = The actual cake you baked 🍰

### 🔄 Basic Docker Workflow

```mermaid
graph LR
    A[Pull Image] --> B[Run Container]
    B --> C[Interact with Container]
    C --> D[Stop Container]
    D --> E[Remove Container]
```

### 🌐 Your First Container

Let's run a simple web server:

```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

**Command breakdown:**

| Flag | Purpose |
|------|---------|
| `docker run` | Creates and starts a new container |
| `-d` | Runs in "detached" mode (background) |
| `-p 8080:80` | Maps port 8080 (host) to port 80 (container) |
| `--name my-nginx` | Gives the container a friendly name |
| `nginx` | The image to use |

Now open your browser and visit: `http://localhost:8080` 🌐

You should see the nginx welcome page! 🎉

### 🛑 Stop and Remove the Container

```bash
docker stop my-nginx
docker rm my-nginx
```

---

# Setting up SQL Server in Docker - The Bulletproof Guide

Since SQL Server isn't natively supported on Ubuntu 24.04, Docker provides the perfect solution! This guide includes battle-tested solutions for common permission issues and volume mounting challenges.

## 🎯 TL;DR - The Robust Setup Command (First time setup if no other sql server exists inside docker container)

**Database Setup**

**First, source the environment variables:**
```bash
source /home/bijoy/Documents/Linux/.env
```

```bash
# The bulletproof command that just works
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -v /home/bijoy/Documents/Data-Engineering/:/opt/analytics \
   -d mcr.microsoft.com/mssql/server:2019-latest
```
**-v /host/path:/container/path  # Docker volume mount**

```bash
# Start sql-server
docker start sql-server
```

```bash
# Connect to sql-server
docker exec -it sql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "$SQLSERVER_PASSWORD" -C
```

**If you like sql server 2022**

```bash
# The Bulletproof SQL Server 2022 Command:
# This is your best bet with 2022
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -e "MSSQL_USER=root" \
   -p 1433:1433 --name sql-server \
   -v /home/bijoy/Documents/Data-Engineering/:/opt/analytics \
   -d mcr.microsoft.com/mssql/server:2022-latest
```
**Everything else identical to 2019 setup!**

## The Container Filesystem Layout:

### INSIDE EVERY LINUX CONTAINER (including SQL Server):
/
├── /opt/           # ← ALREADY EXISTS (for optional software)
├── /usr/           # ← ALREADY EXISTS (for user programs)
├── /var/           # ← ALREADY EXISTS (for variable data)
├── /tmp/           # ← ALREADY EXISTS (for temporary files)
├── /home/          # ← ALREADY EXISTS (for user directories)
└── /bin/           # ← ALREADY EXISTS (for essential binaries)

--------------------------------------------------------------------------------------------------------------

Container Root (/)
├── /opt/               # ← SAFE ZONE for user mounts
│   └── analytics/      # ← YOUR data lives here peacefully  # ← VIRTUAL directory, points to your real files!
├── /var/opt/mssql/     # ← SQL SERVER'S PRIVATE TERRITORY  # ← SQL Server's virtual system files
│   ├── data/           # ← System databases (HANDS OFF!)
│   ├── log/            # ← SQL Server logs
│   └── secrets/        # ← SQL Server secrets
└── /usr/local/         # ← Application binaries

--------------------------------------------------------------------------------------------------------------

## The SQL Server Container Pre-Built Structure:

## Microsoft ALREADY put SQL Server tools in the standard places:
/opt/mssql/                    # ← SQL Server installation
/opt/mssql-tools18/bin/sqlcmd  # ← Command line tools (WHERE THEY BELONG!)
/usr/bin/                      # ← Other system binaries
/var/opt/mssql/                # ← SQL Server's data directory

--------------------------------------------------------------------------------------------------------------

## The Full Picture:

/opt/
├── mssql/              # ← Microsoft put SQL Server here
├── mssql-tools18/      # ← Microsoft put tools here
└── analytics/          # ← WE mounted our data here

--------------------------------------------------------------------------------------------------------------

## 📥 Step 1: Choose Your SQL Server Version

### Recommended: SQL Server 2019 (Battle-Tested)
```bash
# Use 2019 for stability and fewer headaches
docker pull mcr.microsoft.com/mssql/server:2019-latest
```

### Alternative: SQL Server 2022 (If You Must)
```bash
# 2022 has more features but also more permission issues
docker pull mcr.microsoft.com/mssql/server:2022-latest
```

> **🛡️ Why 2019 is Better:**
> - Mature ecosystem with more community solutions
> - Proven stability in production environments
> - Lighter resource usage
> - Fewer breaking changes and permission conflicts

## 🚀 Step 2: Run SQL Server Container

### Basic Setup (No Data Access)
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -d mcr.microsoft.com/mssql/server:2019-latest
```

### Robust Setup (With External Data Access)
```bash
# Mount your entire analytics folder for maximum flexibility
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -v /home/bijoy/Documents/Data-Engineering:/opt/analytics \
   -d mcr.microsoft.com/mssql/server:2019-latest
```

**Command breakdown:**
| Parameter | Purpose |
|-----------|---------|
| `-e "ACCEPT_EULA=Y"` | Accepts the End User License Agreement |
| `-e "SA_PASSWORD=$SQLSERVER_PASSWORD"` | Sets the system administrator password |
| `-p 1433:1433` | Maps SQL Server's default port |
| `--name sql-server` | Names the container for easy reference |
| `-v /path/to/data:/opt/analytics` | Mounts external data (CRITICAL for data projects) |
| `-d` | Runs in background |

> **⚠️ CRITICAL: Volume Mount Path**
> - ✅ **DO:** Mount to `/opt/analytics` (clean separation)
> - ❌ **DON'T:** Mount to `/var/opt/mssql/data/` (conflicts with SQL Server system files)

## 🔐 Password Requirements

The password `$SQLSERVER_PASSWORD` meets SQL Server requirements:

| Requirement | ✅ Status |
|-------------|----------|
| At least 8 characters | ✅ (33 characters) |
| Contains uppercase letters | ✅ (D) |
| Contains lowercase letters | ✅ (ata) |
| Contains numbers | ✅ (2026) |
| Contains special characters | ✅ (-) |

> **🔒 Security Tip:** For production environments, use a stronger, unique password!

## ✅ Step 3: Verify SQL Server is Running

```bash
# Check if container is running
docker ps

# If not running, start it
docker start sql-server

# Stop when needed
docker stop sql-server
```

**Troubleshooting Container Issues:**
```bash
# Check container status (including stopped containers)
docker ps -a

# View container logs to debug startup issues
docker logs sql-server

# Common fix: Remove and recreate if corrupted
docker stop sql-server
docker rm sql-server
# Then run the creation command again
```

## 🔌 Step 4: Connect to SQL Server

### From Command Line (Updated Path)
```bash
# Modern SQL Server uses mssql-tools18
docker exec -it sql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "$SQLSERVER_PASSWORD" -C
```

**Command breakdown:**
- `docker exec` - Execute a command inside a running container
- `-it` - Interactive terminal (lets you type commands)
- `sql-server` - Name of your SQL Server container
- `/opt/mssql-tools18/bin/sqlcmd` - Path to SQL Server command line tool (Note: tools18, not tools)
- `-S localhost` - Server name (localhost since we're inside the container)
- `-U SA` - Username (SA = System Administrator)
- `-P "$SQLSERVER_PASSWORD"` - Password for SA user (quotes for safety)
- `-C` - Trust server certificate (bypasses SSL certificate validation)

### External Tool Connection Details
| Setting | Value |
|---------|-------|
| **Server** | `localhost,1433` |
| **Trust Server Certificate** | ✅ Enabled |
| **Authentication** | SQL Server Authentication |
| **Username** | `SA` |
| **Password** | `$SQLSERVER_PASSWORD` |

## 🗃️ Step 5: Create a Test Database

Once connected via sqlcmd:
```sql
-- Test the connection
SELECT @@VERSION;
GO

-- Create a test database
CREATE DATABASE MyTestDB;
GO

USE MyTestDB;
GO

-- Create and test a table
CREATE TABLE Users (ID INT, Name VARCHAR(50));
GO

INSERT INTO Users VALUES (1, 'John Doe');
GO

SELECT * FROM Users;
GO
```

> **📝 Note:** The `GO` keyword executes the batch of commands above it.

## 📊 Step 6: Access External Data Files

With the volume mount setup, you can now use BULK INSERT to load CSV files:

```sql
-- Example: Loading a CSV file from your mounted analytics folder
BULK INSERT MyTable
FROM '/opt/analytics/Learning/sql/YourProject/datasets/data.csv'
WITH (
    FIRSTROW = 2,           -- Skip header row
    FIELDTERMINATOR = ',',  -- CSV delimiter
    TABLOCK                 -- Table lock for better performance
);
GO
```

**File Path Translation:**
- Host: `/home/bijoy/Documents/Data-Engineering/Learning/sql/YourProject/datasets/data.csv`
- Container: `/opt/analytics/Learning/sql/YourProject/datasets/data.csv`

## 💾 Step 7: Data Persistence Options

### Option 1: Named Volume (System Managed)
```bash
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -v sql-server-data:/var/opt/mssql \
   -v /home/bijoy/Documents/Data-Engineering:/opt/analytics \
   -d mcr.microsoft.com/mssql/server:2019-latest
```

### Option 2: Host Directory (Full Control)
```bash
mkdir -p ~/sql-server-data
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -v ~/sql-server-data:/var/opt/mssql \
   -v /home/bijoy/Documents/Data-Engineering:/opt/analytics \
   -d mcr.microsoft.com/mssql/server:2019-latest
```

## 🔧 Common Issues and Solutions

### Issue 1: Container Exits Immediately
**Symptoms:** Container shows as "Exited" right after creation
**Solution:** Check logs and avoid mounting to `/var/opt/mssql/data/`
```bash
docker logs sql-server
# Usually shows "Access is denied" or permission errors
```

### Issue 2: Permission Denied on BULK INSERT
**Symptoms:** "Cannot bulk load. File does not exist or you don't have file access rights"
**Root Cause:** Using host file paths instead of container paths
**Solution:** Use container paths starting with `/opt/analytics/`

### Issue 3: SQL Server 2022 Permission Issues
**Symptoms:** "Setup FAILED copying system data file" errors
**Solutions:**
1. Use SQL Server 2019 (recommended)
2. Or add `-e "MSSQL_USER=root"` to override security settings

## 📋 Quick Reference Commands

```bash
# Container management
docker start sql-server
docker stop sql-server  
docker restart sql-server
docker rm sql-server

# Connection and access
docker exec -it sql-server bash
docker exec -it sql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "$SQLSERVER_PASSWORD" -C

# File system access
docker exec -it sql-server ls -la /opt/analytics/
docker cp localfile.csv sql-server:/opt/analytics/

# Monitoring
docker ps
docker logs sql-server
docker stats sql-server
```

## 🎯 Best Practices Summary

1. **Use SQL Server 2019** for stability and fewer headaches
2. **Mount data to `/opt/analytics`** not `/var/opt/mssql/data/`
3. **Always use container paths** in SQL BULK INSERT statements
4. **Check logs** when containers fail to start: `docker logs sql-server`
5. **Use quotes around passwords** in command line: `-P "$SQLSERVER_PASSWORD"`
6. **Plan your volume mounts** - mount at project or analytics level for flexibility

> **🏆 Success Metrics:** 
> When properly configured, you should achieve:
> - Sub-second ETL load times for thousands of rows
> - No permission denied errors
> - Seamless access to all your data analytics files
> - Rock-solid container stability

**Happy data warehousing! 🚀📊**

## 🗄️ Restoring SQL Server Backup Files

A comprehensive guide for restoring SQL Server databases from backup files (.bak) using Docker containers. 🐳

### ✅ Prerequisites

- Docker installed and running 🐳
- SQL Server container named `sql-server` already running ⚡
- Database backup files (.bak) available on your local system 💾
- SA password: `$SQLSERVER_PASSWORD` 🔑

### 🚀 Step-by-Step Process

#### 1. 📁 Create Local Backup Directory & Keep .bak Files There

```bash
# Put backup in your host analytics folder
mkdir -p /home/bijoy/Documents/Data-Engineering/backups/
```

**What it does:**
- Creates a new directory called `backups` in your Data-Engineering directory (`~/Documents/Data-Engineering/`)
- The `-p` flag ensures parent directories are created if they don't exist
- If the directory already exists, no error is thrown

#### 2. 🔌 Connect to SQL Server

```bash
docker exec -it sql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "$SQLSERVER_PASSWORD" -C
```

**What it does:** ⚡
- `docker exec -it`: Executes an interactive command in the running container
- `sql-server`: Name of your Docker container
- `/opt/mssql-tools18/bin/sqlcmd`: Path to SQL Server command-line tool
- `-S localhost`: Specifies the server name (localhost since we're inside the container)
- `-U SA`: Connects using the System Administrator account
- `-P "$SQLSERVER_PASSWORD"`: Provides the SA password
- `-C`: Trusts the server certificate (useful for development environments)

### 🔄 Database Restoration Commands

Once connected to SQL Server, execute these T-SQL commands: 💻

#### 🏢 AdventureWorks2022 Database

```sql
# No docker cp needed! It's already mounted!
RESTORE DATABASE [AdventureWorks2022]
FROM DISK = '/opt/analytics/backups/AdventureWorks2022.bak'
WITH MOVE 'AdventureWorks2022' TO '/var/opt/mssql/data/AdventureWorks2022.mdf',
     MOVE 'AdventureWorks2022_log' TO '/var/opt/mssql/data/AdventureWorks2022_log.ldf';
GO
```

**What it does:** 🛠️
- `RESTORE DATABASE`: T-SQL command to restore a database from backup
- `FROM DISK`: Specifies the backup file location
- `WITH MOVE`: Relocates database files to new locations
- First `MOVE` relocates the primary data file (.mdf)
- Second `MOVE` relocates the transaction log file (.ldf)
- `GO`: Batch separator that signals the end of the T-SQL batch

#### 📊 AdventureWorksDW2022 Database

```sql
RESTORE DATABASE [AdventureWorksDW2022]
FROM DISK = '/opt/analytics/backups/AdventureWorksDW2022.bak'
WITH MOVE 'AdventureWorksDW2022' TO '/var/opt/mssql/data/AdventureWorksDW2022.mdf',
     MOVE 'AdventureWorksDW2022_log' TO '/var/opt/mssql/data/AdventureWorksDW2022_log.ldf';
GO
```

**What it does:** 🏪
- Same restore process as above but for the AdventureWorksDW2022 data warehouse database
- DW stands for Data Warehouse, containing analytical data structures

#### 🗃️ MyDatabase

```sql
RESTORE DATABASE [MyDatabase] 
FROM DISK = '/opt/analytics/backups/MyDatabase.bak'
WITH MOVE 'MyDatabase' TO '/var/opt/mssql/data/MyDatabase.mdf',
     MOVE 'MyDatabase_log' TO '/var/opt/mssql/data/MyDatabase_log.ldf';
GO
```

**What it does:** 🎯
- Restores a custom database named "MyDatabase"
- Follows the same pattern as other database restorations

#### 💰 SalesDB Database

```sql
RESTORE DATABASE [SalesDB]
FROM DISK = '/opt/analytics/backups/SalesDB.bak'
WITH MOVE 'SalesDB' TO '/var/opt/mssql/data/SalesDB.mdf',
     MOVE 'SalesDB_log' TO '/var/opt/mssql/data/SalesDB_log.ldf';
GO
```

**What it does:** 📈
- Restores a sales-related database
- Uses the same restore pattern with appropriate file naming

#### 🚪 Exit SQL Server Connection

```sql
EXIT
```

**What it does:** 👋
- Closes the sqlcmd session and returns to the container's command prompt
- Alternative: Use `QUIT` command or press Ctrl+C

### 🔍 Verification Commands

After restoration, verify your databases using these queries: ✅

#### 📋 Simple Database List

```sql
SELECT name FROM sys.databases;
GO
```

**What it does:** 📊
- Queries the system catalog view `sys.databases`
- Returns only the database names
- Shows all databases including system databases (master, model, msdb, tempdb)

#### 📋 Detailed Database Information

```sql
SELECT 
    name,
    database_id,
    create_date,
    collation_name,
    state_desc
FROM sys.databases;
GO
```

**What it does:** 📊
- Provides comprehensive information about each database:
  - `name`: Database name
  - `database_id`: Unique identifier for the database
  - `create_date`: When the database was created
  - `collation_name`: Character set and sorting rules
  - `state_desc`: Current state (ONLINE, OFFLINE, etc.)

#### 📑 Alternative Database List

```sql
EXEC sp_databases;
```

**What it does:** 🏃‍♂️
- Executes the system stored procedure `sp_databases`
- Returns database names and their sizes
- Provides a quick overview of all databases

### 📌 Important Notes

- **File Paths**: All paths inside the container use Linux-style forward slashes 🐧
- **Permissions**: The SA account has full administrative privileges 🔐
- **Security**: The password shown is for development purposes only ⚠️
- **Backup Location**: Backup files are stored in `/opt/analytics/backups/` 📂
- **Database Files**: Restored databases are placed in `/var/opt/mssql/data/` 💾

### 🛠️ Troubleshooting Tips

1. **File Not Found** 🔍: Verify the backup file path and ensure files were copied correctly
2. **Permission Issues** 🚫: Ensure the SQL Server service account has read access to backup files
3. **Database Already Exists** ⚠️: Use `DROP DATABASE` or `WITH REPLACE` option if needed
4. **Space Issues** 💾: Check available disk space in the container
5. **Connection Problems** 🔌: Verify container is running and credentials are correct

### 🔒 Security Considerations

- Change the SA password for production environments 🔑
- Use Windows Authentication or create specific database users 👥
- Implement proper backup encryption for sensitive data 🔐
- Regular security updates for the SQL Server container image 🛡️

---

## Installing Portainer for Docker GUI Management

Portainer provides a beautiful web-based GUI to manage Docker containers, images, and volumes easily.

### 💾 Step 1: Create Portainer Volume

```bash
docker volume create portainer_data
```

### 🚀 Step 2: Install Portainer

```bash
docker run -d -p 9000:9000 --name portainer \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce
```

**Command breakdown:**

| Parameter | Purpose |
|-----------|---------|
| `-d` | Runs in background |
| `-p 9000:9000` | Maps port 9000 for web access |
| `--name portainer` | Names the container |
| `-v /var/run/docker.sock:/var/run/docker.sock` | Gives Portainer access to Docker daemon |
| `-v portainer_data:/data` | Persists Portainer configuration |
| `portainer/portainer-ce` | Portainer Community Edition image |

### 🌐 Step 3: Access Portainer

1. Open your web browser
2. Navigate to: `http://localhost:9000`
3. Create your admin account on first visit
4. Select **"Docker"** as the environment to manage

### 🎯 Portainer Features

Through Portainer's web interface, you can:

- 🚀 Start/stop/restart containers
- 📋 View container logs
- 🖼️ Manage images and volumes
- 📊 Monitor resource usage
- 💻 Access container terminals
- 🚀 Deploy applications from templates

---

### 🔌 Step 4: Connect to SQL Server Container from any SQL Management Tools

1. Click **"New Connection"**
2. Fill in connection details:

| Field | Value |
|-------|--------|
| **Server** | `localhost,1433` |
| **Authentication type** | SQL Login |
| **User name** | `SA` |
| **Password** | `$SQLSERVER_PASSWORD` |
| **Database** | `<Default>` |

3. Click **"Connect"**

> **🎉 Success!** You should now see your SQL Server instance in the sidebar.

---

## Quick Reference Commands

### 🗄️ SQL Server Management

```bash
# Start SQL Server with backup support
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=$SQLSERVER_PASSWORD" \
   -p 1433:1433 --name sql-server \
   -v ~/sql-backups:/var/opt/mssql/backup \
   -v sql-server-data:/var/opt/mssql \
   -d mcr.microsoft.com/mssql/server:2022-latest

# Copy backup files
cp /path/to/backup.bak ~/sql-backups/

# Access container bash
docker exec -it sql-server /bin/bash

# View container logs
docker logs sql-server

# Connect with sqlcmd
docker exec -it sql-server /opt/mssql-tools18/bin/sqlcmd -S localhost -U SA -P "$SQLSERVER_PASSWORD" -C
```

### 🎛️ Portainer Management

```bash
# Access Portainer web interface
# Open browser: http://localhost:9000

# Restart Portainer
docker restart portainer

# Update Portainer
docker pull portainer/portainer-ce:latest
docker stop portainer
docker rm portainer
# Then run the installation command again
```

---

## Essential Docker Commands

### 📦 Container Management

```bash
# List running containers
docker ps (ps = processes status)

# List all containers (including stopped)
docker ps -a

# Start a stopped container
docker start <container-name>

# Stop a running container
docker stop <container-name>

# Restart a container
docker restart <container-name>

# Remove a container
docker rm <container-name>

# Remove a running container forcefully
docker rm -f <container-name>

# View container logs
docker logs <container-name>

# Follow logs in real-time
docker logs -f <container-name>

# Execute commands in running container
docker exec -it <container-name> /bin/bash

# Copy files to/from container
docker cp <container-name>:/path/to/file /host/path
docker cp /host/path <container-name>:/path/to/file
```

### 🖼️ Image Management

```bash
# List local images
docker images

# Pull image from Docker Hub
docker pull <image-name>:<tag>

# Remove an image
docker rmi <image-name>:<tag>

# Build image from Dockerfile
docker build -t <my-image>:<tag> .

# Search for images on Docker Hub
docker search <search-term>

# View image history
docker history <image-name>

# Inspect image details
docker inspect <image-name>
```

### 💾 Volume Management

```bash
# List volumes
docker volume ls

# Create a volume
docker volume create <volume-name>

# Remove a volume
docker volume rm <volume-name>

# Inspect volume details
docker volume inspect <volume-name>

# Remove unused volumes
docker volume prune
```

### 🧹 System Cleanup

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove unused volumes
docker volume prune

# Remove unused networks
docker network prune

# Remove everything unused (be careful!)
docker system prune -a

# Show disk usage
docker system df
```

---

## Troubleshooting

### 🔧 Common Issues and Solutions

#### ❌ "Permission denied" when running Docker commands

**Solution:**
```bash
# Check if you're in docker group
groups $USER

# If docker is not listed, add yourself
sudo usermod -aG docker $USER

# Restart your computer
sudo reboot
```

#### ❌ "Cannot connect to the Docker daemon"

**Solution:**
```bash
# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Check Docker status
sudo systemctl status docker
```

#### ❌ SQL Server container exits immediately

**Solution:**
```bash
# Check container logs
docker logs sql-server

# Common causes:
# 1. Password doesn't meet requirements
# 2. Port 1433 already in use
# 3. Insufficient memory
```

#### ❌ "Port already in use"

**Solution:**
```bash
# Find what's using the port
sudo netstat -tulpn | grep :1433

# Use a different port
docker run -p 1434:1433 ...

# Or stop the service using that port
sudo systemctl stop <service-name>
```

#### ❌ Container runs out of space

**Solution:**
```bash
# Clean up unused resources
docker system prune -a

# Check disk usage
docker system df

# Remove large unused images
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}" | sort -k3 -h
```

### 🔍 Debugging Commands

```bash
# Check Docker version
docker --version

# Check system information
docker info

# Monitor container resource usage
docker stats

# Inspect container configuration
docker inspect <container-name>

# View container processes
docker top <container-name>

# Check Docker daemon logs
journalctl -u docker.service
```

---

## Best Practices

### 🎯 Development Best Practices

1. **📌 Always specify image tags** instead of using `latest`
   ```bash
   # Good
   docker run nginx:1.21-alpine
   
   # Avoid
   docker run nginx:latest
   ```

2. **💾 Use volumes for persistent data**
   ```bash
   # Always use volumes for databases
   docker run -v mydata:/var/lib/mysql mysql:8.0
   ```

3. **🧹 Regularly clean up unused resources**
   ```bash
   # Weekly cleanup routine
   docker system prune -a
   ```

4. **📝 Use meaningful names for containers and volumes**
   ```bash
   # Good
   docker run --name web-server nginx
   
   # Avoid
   docker run nginx
   ```

### 🚀 Performance Best Practices

5. **📊 Monitor resource usage**
   ```bash
   docker stats
   ```

6. **🔄 Keep images updated for security patches**
   ```bash
   docker pull nginx:latest
   ```

7. **📁 Use `.dockerignore` files** to exclude unnecessary files

8. **👤 Don't run containers as root** when possible

### 🔒 Security Best Practices

9. **🔐 Use strong passwords** for databases

10. **🌐 Limit port exposure** to only necessary ports

11. **📋 Regularly audit running containers**
    ```bash
    docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
    ```

---

## Next Steps

Now that you have a complete Docker setup with SQL Server and Portainer, here's what you can explore next:

### 🚀 Immediate Next Steps

1. **🎛️ Master Portainer** - Use the web interface for visual Docker management
2. **💾 Practice database restoration** - Try restoring various backup files
3. **🖥️ Explore Azure Data Studio** - Learn advanced database management features
4. **📊 Monitor performance** - Use Portainer to track container metrics

### 🔄 Advanced Learning

5. **🐙 Learn Docker Compose** - Manage multi-container applications
6. **🌐 Explore Docker Hub** - Discover useful pre-built images
7. **🏗️ Create custom images** - Build your own Dockerfiles
8. **☁️ Cloud deployment** - Deploy containers to AWS, Azure, or GCP

### 📚 Additional Resources

- **📖 Official Docker Documentation**: https://docs.docker.com/
- **🎓 Docker Learning Resources**: https://docs.docker.com/get-started/
- **🎯 SQL Server on Docker**: https://docs.microsoft.com/sql/linux/sql-server-linux-docker-container-deployment
- **🎛️ Portainer Documentation**: https://docs.portainer.io/

---

## 🎉 Congratulations!

You've successfully set up a complete Docker development environment with:

- ✅ Docker Engine installed and configured
- ✅ SQL Server running in a container
- ✅ Backup and restore capabilities
- ✅ Portainer for visual container management
- ✅ Best practices and troubleshooting knowledge

**You're now ready to containerize your applications and manage databases like a pro!** 🚀

---

> **📌 Keep this guide handy!** Bookmark it for future reference and don't hesitate to revisit any section when you need to reinstall or troubleshoot these applications.

**Happy Dockerizing!** 🐳✨

*This comprehensive guide was created to take you from Docker beginner to confident user. Keep learning and exploring the amazing world of containerization!*
