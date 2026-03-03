# The Ultimate Linux Commands Holy Grail 🐧⚡
*Every command you'll ever need, decoded and explained*

---

## 1️⃣ SYSTEM INFORMATION 🖥️

| Command | Meaning | What It Does | Pro Tip |
|---------|---------|--------------|---------|
| `uname -a` | Unix Name - All | Complete system info dump | Use `-r` for just kernel version |
| `uname -r` | Unix Name - Release | Just the kernel version | Perfect for compatibility checks |
| `cat /etc/os-release` | Show OS release info | Distribution name, version, codename | Replaced old `/etc/issue` |
| `lsb_release -a` | Linux Standard Base release | Detailed distro info | Ubuntu/Debian specific |
| `uptime` | System uptime | How long running + load average | Load should be < # of CPU cores |
| `hostname` | Host name | Your computer's network name | |
| `hostname -I` | Host name - IP addresses | All local IP addresses | Capital I, not lowercase L! |
| `hostname -i` | Host name - primary IP | Main IP address only | |
| `last reboot` | Last reboot times | System reboot history | Great for uptime bragging |
| `date` | Current date/time | What time is it? | Use `date -u` for UTC |
| `cal` | Calendar | Shows current month calendar | `cal 2024` for full year |
| `w` | Who and what | Who's logged in and what they're doing | Better than `who` |
| `users` | Current users | Just usernames of logged in users | Simple list format |

---

## 2️⃣ HARDWARE INFORMATION 🔧

| Command | Meaning | What It Does | When to Use |
|---------|---------|--------------|-------------|
| `lscpu` | List CPU | CPU info in readable format | Quick CPU overview |
| `cat /proc/cpuinfo` | CPU info file | Detailed per-core CPU info | Deep CPU analysis |
| `cat /proc/meminfo` | Memory info file | Detailed RAM information | Memory troubleshooting |
| `free -h` | Free memory - Human | RAM usage in GB/MB | `-m` for MB, `-g` for GB |
| `lshw` | List Hardware | Complete hardware inventory | Needs sudo, very detailed |
| `lshw -short` | List Hardware - Short | Hardware summary | Quick hardware overview |
| `hwinfo --short` | Hardware Info - Short | Another hardware lister | Alternative to lshw |
| `lspci` | List PCI devices | PCI devices (graphics, network, etc.) | |
| `lspci -tv` | List PCI - Tree View | PCI devices in tree format | Easier to read |
| `lsusb` | List USB devices | Everything on USB ports | Mouse, keyboard, drives |
| `lsblk` | List Block devices | All storage devices | Shows mount points too |
| `fdisk -l` | List disk partitions | All disk partitions | Needs sudo |
| `df -h` | Disk Free - Human | Filesystem space usage | Most important disk command |
| `df -i` | Disk Free - Inodes | Inode usage (file count limits) | When you can't create files |
| `dmesg` | Display kernel MESSaGes | Kernel ring buffer | Hardware detection issues |
| `dmesg | tail` | Last kernel messages | Recent kernel messages | First stop for hardware problems |
| `dmidecode` | DMI decode | Hardware info from BIOS | Detailed system specs |

---

## 3️⃣ PERFORMANCE MONITORING 📊

| Command | Meaning | What It Does | Best Use Case |
|---------|---------|--------------|---------------|
| `top` | Table Of Processes | Process monitor (classic) | When htop unavailable |
| `htop` | Human TOP | Interactive process viewer | Always prefer over top |
| `atop` | Advanced TOP | System & process monitor | Most comprehensive |
| `ps` | Process Status | Show running processes | |
| `ps aux` | Process Status - All Users eXtended | All processes, BSD style | Most common ps usage |
| `ps -ef` | Process Status - Extended Format | All processes, System V style | Alternative to ps aux |
| `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem` | Custom ps format | Top memory users | Finding memory hogs |
| `pstree` | Process tree | Processes in tree format | See parent-child relationships |
| `pgrep processname` | Process GREP | Find process IDs by name | Better than ps \| grep |
| `pidof processname` | PID of process | Get PID of running process | Simple PID lookup |
| `jobs` | Background jobs | Your background processes | |
| `nohup command &` | No HangUP | Run command immune to hangups | For long-running tasks |
| `screen` | Terminal multiplexer | Persistent terminal sessions | Sessions survive disconnection |
| `tmux` | Terminal MUltipleXer | Modern screen alternative | Better than screen |
| `iostat 1` | I/O STATistics | Disk I/O stats every second | Disk performance monitoring |
| `iotop` | I/O TOP | Real-time disk I/O by process | Find disk-heavy processes |
| `vmstat 1` | Virtual Memory STATistics | Memory/CPU stats every second | System performance overview |
| `sar` | System Activity Reporter | Historical system stats | Part of sysstat package |
| `mpstat 1` | Multi-Processor STATistics | CPU usage per core | Multi-core CPU monitoring |
| `netstat -tulpn` | NETwork STATistics | Network connections & ports | See what's listening |
| `ss -tulpn` | Socket Statistics | Modern netstat replacement | Faster than netstat |
| `lsof` | List Open Files | All open files on system | Everything is a file in Linux |
| `lsof -u username` | List Open Files - User | Files opened by specific user | User activity monitoring |
| `lsof -i :80` | List Open Files - Internet port | What's using port 80 | Port troubleshooting |
| `watch command` | Execute command repeatedly | Monitor command output | `watch -n 2 df -h` |
| `tail -f /var/log/syslog` | Follow syslog | Real-time system log | Live log monitoring |
| `tail -100 /var/log/syslog` | Last 100 syslog lines | Recent system messages | Quick log check |

---

## 4️⃣ USER MANAGEMENT 👥

| Command | Meaning | What It Does | Security Note |
|---------|---------|--------------|---------------|
| `whoami` | Who Am I | Your current username | |
| `who` | Who's logged in | All logged-in users | |
| `w` | Who and What | Users and their activities | More detailed than who |
| `id` | Identity | Your UID, GID, and groups | Permission troubleshooting |
| `id username` | User's identity | Another user's IDs | |
| `last` | Last logins | Recent login history | Security auditing |
| `lastlog` | Last log entries | All users' last login times | Find inactive accounts |
| `finger username` | User information | Detailed user info | May need installation |
| `groups` | Your groups | Groups you belong to | |
| `groups username` | User's groups | Another user's groups | |
| `getent passwd` | Get entries - passwd | All system users | Includes system accounts |
| `getent group` | Get entries - group | All system groups | |
| `useradd -m username` | Add user with home | Create user + home directory | -m creates home dir |
| `useradd -c "Full Name" -m username` | Add user with comment | Create user with full name | |
| `userdel username` | Delete user | Remove user account | Doesn't remove home dir |
| `userdel -r username` | Delete user - Remove home | Remove user + home directory | Complete removal |
| `usermod -aG groupname username` | Modify user - append Group | Add user to group | -a is crucial (append) |
| `usermod -l newname oldname` | Modify user - Login name | Rename user account | |
| `passwd` | Password | Change your password | |
| `passwd username` | Change user password | Change another user's password | Needs sudo |
| `chage -l username` | CHange AGE - List | Password aging info | When password expires |
| `groupadd groupname` | Add group | Create new group | |
| `groupdel groupname` | Delete group | Remove group | |
| `gpasswd -a username groupname` | Group password - Add | Add user to group | Alternative to usermod |
| `gpasswd -d username groupname` | Group password - Delete | Remove user from group | |
| `su` | Substitute User | Switch to root | Needs root password |
| `su username` | Switch to user | Switch to another user | |
| `sudo -i` | Switch to root with environment | Root login shell | Recommended over su |
| `sudo -s` | Switch to root shell | Root shell, keep environment | |
| `sudo -u username command` | Run as user | Execute command as another user | |
| `visudo` | Edit sudoers file | Safely edit sudo configuration | Only way to edit sudoers |

---

## 5️⃣ FILE & DIRECTORY OPERATIONS 📁

| Command | Meaning | What It Does | Pro Tip |
|---------|---------|--------------|---------|
| `ls` | List directory contents | Show directory contents | |
| `ls -l` | List - Long | Detailed file listing | Shows permissions, size, date |
| `ls -a` | List - All | Include hidden files (.files) | Not Hollywood LA! 😄 |
| `ls -la` | List - Long All | Detailed + hidden files | Most common combo |
| `ls -lah` | List - Long All Human | + human readable sizes | Ultimate ls command |
| `ls -lt` | List - Long Time | Sort by modification time | Find recently changed files |
| `ls -lS` | List - Long Size | Sort by file size | Find largest files |
| `ls -lR` | List - Long Recursive | List subdirectories too | See everything |
| `pwd` | Print Working Directory | Show current directory | Where am I? |
| `cd` | Change Directory | Go to home directory | |
| `cd ..` | Go up one level | Parent directory | |
| `cd -` | Go to previous directory | Like browser back button | Super useful! |
| `cd ~` | Go to home | Home directory | Same as just `cd` |
| `pushd /path` | Push directory | Save current, go to new | Directory stack |
| `popd` | Pop directory | Return to saved directory | |
| `dirs` | Show directory stack | See pushd/popd stack | |
| `mkdir dirname` | Make Directory | Create directory | |
| `mkdir -p path/to/dir` | Make Directory - Parents | Create nested directories | Creates all missing parents |
| `rmdir dirname` | Remove Directory | Delete empty directory | Only works if empty |
| `rm -rf dirname` | Remove - Recursive Force | Delete directory + contents | DANGEROUS! Double-check! |
| `cp file1 file2` | Copy | Copy file | |
| `cp -r dir1 dir2` | Copy - Recursive | Copy directory | |
| `cp -i file1 file2` | Copy - Interactive | Ask before overwriting | Safety first |
| `cp -p file1 file2` | Copy - Preserve | Keep timestamps/permissions | |
| `mv file1 file2` | Move | Move/rename file | Also used for renaming |
| `mv *.txt /backup/` | Move with wildcards | Move all .txt files | Wildcards are powerful |
| `rename 's/old/new/' *.txt` | Rename with regex | Batch rename files | Perl-style regex |
| `rm file` | Remove | Delete file | No recycle bin! |
| `rm -i file` | Remove - Interactive | Ask before deleting | Recommended for safety |
| `rm -f file` | Remove - Force | Force delete | Ignores warnings |
| `shred -vfz -n 3 file` | Secure delete | Overwrite file 3 times | For sensitive data |
| `touch file` | Create empty file | Create file or update timestamp | |
| `stat file` | File statistics | Detailed file information | Timestamps, permissions, size |
| `file filename` | Identify file type | What kind of file is this? | Works without extensions |
| `du -sh dirname` | Disk Usage - Summary Human | Directory size | How big is this folder? |
| `du -ah dirname` | Disk Usage - All Human | Size of all files in directory | Find space hogs |
| `ncdu` | NCurses Disk Usage | Interactive disk usage | Visual disk usage explorer |

---

## 6️⃣ FILE VIEWING & EDITING 📖

| Command | Meaning | What It Does | When to Use |
|---------|---------|--------------|-------------|
| `cat file` | ConCATenate | Display entire file | Small files only |
| `cat -n file` | Cat with line Numbers | Show file with line numbers | |
| `tac file` | Reverse cat | Display file in reverse | cat backwards! |
| `less file` | Pager (less than more) | View file page by page | Large files |
| `more file` | Pager | Older file pager | Use less instead |
| `head file` | First lines | Show first 10 lines | |
| `head -n 20 file` | Head - N lines | Show first 20 lines | |
| `tail file` | Last lines | Show last 10 lines | |
| `tail -n 20 file` | Tail - N lines | Show last 20 lines | |
| `tail -f file` | Tail - Follow | Follow file as it grows | Live log monitoring |
| `tail -F file` | Tail - Follow name | Follow file even if recreated | Better for log rotation |
| `zcat file.gz` | Compressed cat | View gzipped file | No need to decompress |
| `zless file.gz` | Compressed less | Page through gzipped file | |
| `nano file` | Simple text editor | Easy-to-use editor | Good for beginners |
| `vim file` | Vi IMproved | Powerful text editor | Learning curve steep |
| `emacs file` | Editor | Another powerful editor | The "other" editor |
| `gedit file` | GNOME editor | GUI text editor | If you have desktop |
| `hexdump -C file` | Hex dump - Canonical | View file in hexadecimal | Binary file analysis |
| `od -c file` | Octal Dump - Character | View file in octal/char | Alternative to hexdump |
| `strings file` | Extract strings | Text strings from binary files | Find text in binaries |

---

## 7️⃣ FILE PERMISSIONS & OWNERSHIP 🔐

| Command | Meaning | What It Does | Common Usage |
|---------|---------|--------------|--------------|
| `chmod 755 file` | CHange MODe | Set permissions to rwxr-xr-x | Executable files |
| `chmod 644 file` | Change mode | Set permissions to rw-r--r-- | Regular files |
| `chmod +x file` | Add execute permission | Make file executable | Scripts |
| `chmod -x file` | Remove execute permission | Remove execute permission | |
| `chmod u+w file` | User + write | Give owner write permission | |
| `chmod g-r file` | Group - read | Remove group read permission | |
| `chmod o=r file` | Other = read | Set other permissions to read only | |
| `chmod -R 755 directory` | Recursive chmod | Apply permissions to all files/dirs | |
| `chown user file` | CHange OWNer | Change file owner | |
| `chown user:group file` | Change owner:group | Change owner and group | |
| `chown :group file` | Change group only | Change just the group | |
| `chown -R user:group dir` | Recursive chown | Change ownership recursively | |
| `chgrp group file` | CHange GRouP | Change file group | |
| `umask` | User MASK | Show default permissions mask | |
| `umask 022` | Set umask | Set default permission mask | 022 = 755 for dirs, 644 for files |
| `getfacl file` | Get File ACL | Show extended permissions | Advanced permissions |
| `setfacl -m u:user:rw file` | Set File ACL | Set extended permissions | Fine-grained control |

**Permission Number Quick Reference:**
- **7** = rwx (4+2+1) - **6** = rw- (4+2+0) - **5** = r-x (4+0+1) - **4** = r-- (4+0+0)
- **755** = Owner: all, Group/Other: read+execute (typical for executables)
- **644** = Owner: read+write, Group/Other: read only (typical for files)
- **777** = Everyone can do everything (⚠️ DANGEROUS!)

---

## 8️⃣ PROCESS MANAGEMENT 🎭

| Command | Meaning | What It Does | Use Case |
|---------|---------|--------------|----------|
| `ps aux` | Process Status | All processes | System overview |
| `ps -ef` | Process Status Extended | All processes, different format | Alternative format |
| `ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu` | Custom process list | Top CPU users | Performance troubleshooting |
| `pgrep firefox` | Process GREP | Find PIDs by process name | Better than ps \| grep |
| `pkill firefox` | Process KILL | Kill processes by name | |
| `killall firefox` | Kill all processes | Kill all instances | |
| `kill PID` | Terminate process | Kill process by PID | Polite termination |
| `kill -9 PID` | Force kill | Force terminate process | Nuclear option |
| `kill -STOP PID` | Pause process | Suspend process | |
| `kill -CONT PID` | Continue process | Resume suspended process | |
| `kill -HUP PID` | Hang UP signal | Restart/reload process | Config reload |
| `xkill` | X Window kill | Click to kill GUI program | GUI troubleshooting |
| `program &` | Background execution | Run program in background | |
| `jobs` | List background jobs | Show your background processes | |
| `fg` | Foreground | Bring recent job to foreground | |
| `fg %1` | Foreground job 1 | Bring specific job to foreground | |
| `bg` | Background | Resume paused job in background | |
| `bg %1` | Background job 1 | Resume specific job in background | |
| `nohup command &` | No HangUP | Run immune to hangups | Long-running tasks |
| `disown` | Disown job | Detach job from shell | Job survives shell exit |
| `screen -S sessionname` | Start named screen | Create persistent session | |
| `screen -r sessionname` | Resume screen | Reconnect to session | |
| `tmux new -s sessionname` | New tmux session | Create named tmux session | |
| `tmux attach -t sessionname` | Attach tmux | Reconnect to tmux session | |
| `systemctl start service` | Start systemd service | Start system service | |
| `systemctl stop service` | Stop systemd service | Stop system service | |
| `systemctl restart service` | Restart service | Restart system service | |
| `systemctl status service` | Service status | Check service status | |
| `systemctl enable service` | Enable service | Start service at boot | |
| `systemctl disable service` | Disable service | Don't start at boot | |
| `service servicename start` | Start SysV service | Old-style service management | Legacy systems |

---

## 9️⃣ SEARCH & TEXT PROCESSING 🔍

| Command | Meaning | What It Does | Power Usage |
|---------|---------|--------------|-------------|
| `grep pattern file` | Global Regular Expression Print | Search for text in files | |
| `grep -i pattern file` | Case-Insensitive grep | Ignore case when searching | |
| `grep -r pattern directory` | Recursive grep | Search in all files in directory | |
| `grep -v pattern file` | Invert match | Show lines NOT containing pattern | |
| `grep -n pattern file` | Line numbers | Show line numbers with matches | |
| `grep -c pattern file` | Count matches | Count matching lines | |
| `grep -l pattern *.txt` | List filenames | Show only filenames with matches | |
| `grep -A 3 -B 3 pattern file` | After/Before context | Show 3 lines before/after match | Context searching |
| `egrep 'pattern1\|pattern2' file` | Extended grep | Search for multiple patterns | |
| `fgrep 'literal.string' file` | Fixed string grep | Search for literal string (no regex) | |
| `zgrep pattern file.gz` | Compressed grep | Search in gzipped files | |
| `find . -name "*.txt"` | Find files by name | Search for files | |
| `find . -type f -name "*.log"` | Find files by type and name | Find regular files only | |
| `find . -size +100M` | Find large files | Files bigger than 100MB | Disk cleanup |
| `find . -mtime -7` | Find by modification time | Files modified in last 7 days | |
| `find . -user username` | Find by owner | Files owned by user | |
| `find . -perm 777` | Find by permissions | Files with specific permissions | Security audit |
| `find . -name "*.tmp" -delete` | Find and delete | Delete all .tmp files | Cleanup operations |
| `locate filename` | Locate file | Fast file search using database | |
| `updatedb` | Update locate database | Refresh locate database | Run as root |
| `which command` | Which executable | Show path of command | |
| `whereis command` | Where is command | Show binary, source, manual paths | |
| `type command` | Command type | Show how command is interpreted | |
| `sort file` | Sort lines | Sort file contents | |
| `sort -n file` | Numeric sort | Sort numerically | |
| `sort -r file` | Reverse sort | Sort in reverse order | |
| `sort -u file` | Unique sort | Sort and remove duplicates | |
| `uniq file` | Unique lines | Remove consecutive duplicate lines | Use after sort |
| `uniq -c file` | Count unique | Count occurrences of each line | |
| `cut -d: -f1 /etc/passwd` | Cut fields | Extract specific columns | |
| `cut -c1-10 file` | Cut characters | Extract character positions | |
| `awk '{print $1}' file` | AWK text processor | Print first field | Powerful text processing |
| `awk -F: '{print $1}' /etc/passwd` | AWK with field separator | Use : as separator | |
| `sed 's/old/new/' file` | Stream EDitor | Replace text | |
| `sed 's/old/new/g' file` | Global replace | Replace all occurrences | |
| `sed -i 's/old/new/g' file` | In-place edit | Modify file directly | ⚠️ No backup! |
| `tr 'a-z' 'A-Z' < file` | TRanslate characters | Convert lowercase to uppercase | |
| `tr -d '\r' < file` | Delete characters | Remove carriage returns | Windows → Unix |
| `wc file` | Word Count | Count lines, words, characters | |
| `wc -l file` | Line count | Count just lines | |
| `wc -w file` | Word count | Count just words | |
| `wc -c file` | Character count | Count just characters | |

---

## 🔟 NETWORKING 🌐

| Command | Meaning | What It Does | Common Usage |
|---------|---------|--------------|--------------|
| `ip a` | IP Address show all | Show all network interfaces | Modern ifconfig replacement |
| `ip addr show dev eth0` | Show specific interface | Details for eth0 only | |
| `ip route` | Show routing table | Network routing information | |
| `ip link show` | Show network links | Physical network interfaces | |
| `ifconfig` | Interface CONFIGuration | Show/configure network interfaces | Old but still works |
| `ifconfig eth0 up` | Bring interface up | Enable network interface | |
| `ifconfig eth0 down` | Bring interface down | Disable network interface | |
| `ping host` | Packet INternet Groper | Test network connectivity | |
| `ping -c 4 host` | Ping count | Send only 4 ping packets | |
| `ping6 host` | IPv6 ping | Test IPv6 connectivity | |
| `mtr host` | My TraceRoute | Continuous traceroute | Better than traceroute |
| `traceroute host` | Trace route | Show network path to host | |
| `tracepath host` | Trace path | Alternative to traceroute | |
| `nslookup domain` | Name Server LOOKUP | DNS lookup (interactive) | |
| `dig domain` | Domain Information Groper | DNS lookup (better than nslookup) | |
| `dig @8.8.8.8 domain` | Dig with specific DNS | Query specific DNS server | |
| `dig -x IP` | Reverse DNS lookup | IP to hostname | |
| `host domain` | Host lookup | Simple DNS lookup | |
| `whois domain` | Domain registration info | Who owns this domain? | |
| `netstat -tulpn` | NETwork STATistics | Show listening ports/connections | |
| `netstat -i` | Network interface stats | Interface packet statistics | |
| `ss -tulpn` | Socket Statistics | Modern netstat replacement | Faster than netstat |
| `ss -s` | Socket summary | Connection summary | |
| `lsof -i` | List Open Files - Internet | Network connections | |
| `lsof -i :80` | Network connections on port 80 | What's using port 80? | |
| `arp -a` | Address Resolution Protocol | Show ARP table (IP→MAC mapping) | |
| `route -n` | Show routing table | Network routes (numeric) | |
| `iwconfig` | Wireless config | Wireless interface configuration | |
| `iwlist scan` | Wireless list scan | Scan for WiFi networks | |
| `ethtool eth0` | Ethernet tool | Network hardware settings | |
| `netcat -l 1234` | Network cat - listen | Listen on port 1234 | Testing/debugging |
| `nc -zv host 80` | Test port connectivity | Check if port 80 is open | Port scanning |
| `tcpdump -i eth0` | Capture packets on eth0 | Network packet capture | |
| `tcpdump 'port 80'` | Capture HTTP traffic | Capture only port 80 traffic | |
| `wireshark` | GUI packet analyzer | Graphical network analysis | |
| `iftop` | Interface TOP | Real-time bandwidth usage | |
| `nethogs` | Network hogs | Bandwidth usage by process | |
| `nmap host` | Network MAPper | Port scanner | Security testing |
| `wget url` | Web GET | Download files from web | |
| `wget -r url` | Recursive wget | Download entire websites | |
| `curl url` | Client URL | Transfer data to/from servers | More versatile than wget |
| `curl -I url` | HTTP headers only | Just get HTTP headers | |
| `curl -X POST -d "data" url` | HTTP POST | Send POST request | API testing |
| `lynx url` | Text-based web browser | Browse web in terminal | |
| `links url` | Another text browser | Alternative to lynx | |

---

## 1️⃣1️⃣ FILE TRANSFERS 📤📥

| Command | Meaning | What It Does | Best For |
|---------|---------|--------------|----------|
| `scp file user@host:/path/` | Secure CoPy to remote | Copy file to remote server | Simple file transfers |
| `scp user@host:/path/file .` | Copy from remote | Copy file from remote server | |
| `scp -r dir/ user@host:/path/` | Recursive scp | Copy directory to remote | |
| `scp -P 2222 file user@host:/path/` | SCP with custom port | Use different SSH port | Non-standard SSH ports |
| `rsync -av source/ dest/` | Remote SYNC - Archive Verbose | Sync directories efficiently | Best for backups |
| `rsync -avz local/ user@host:remote/` | rsync with compression | Sync over network with compression | Network transfers |
| `rsync --delete source/ dest/` | rsync with delete | Remove files not in source | Mirror directories |
| `rsync --dry-run source/ dest/` | rsync preview | Show what would be transferred | Test before real sync |
| `rsync --progress source/ dest/` | rsync with progress | Show transfer progress | Large transfers |
| `rsync --exclude="*.tmp" source/ dest/` | rsync with exclusions | Skip certain files | Selective sync |
| `sftp user@host` | Secure FTP | Interactive secure file transfer | Interactive transfers |
| `ftp host` | File Transfer Protocol | Traditional FTP (insecure) | Legacy systems only |
| `ftps host` | FTP Secure | Secure FTP | Better than plain FTP |
| `lftp host` | Enhanced FTP client | Advanced FTP client | Complex FTP operations |
| `rcp file user@host:/path/` | Remote CoPy | Remote copy (insecure) | Obsolete, use scp |
| `tar czf - directory/ \| ssh user@host 'tar xzf - -C /path/'` | Tar over SSH | Stream tar archive over SSH | Efficient directory transfer |

---

## 1️⃣2️⃣ ARCHIVES & COMPRESSION 📦

| Command | Meaning | What It Does | File Extension |
|---------|---------|--------------|----------------|
| `tar cf archive.tar files/` | Tape ARchive - Create File | Create uncompressed archive | .tar |
| `tar xf archive.tar` | Extract from archive | Extract tar archive | |
| `tar tf archive.tar` | List tar contents | List files in archive without extracting | |
| `tar czf archive.tar.gz files/` | Create gzipped tar | Create compressed archive | .tar.gz, .tgz |
| `tar xzf archive.tar.gz` | Extract gzipped tar | Extract compressed archive | |
| `tar cjf archive.tar.bz2 files/` | Create bzip2 tar | Create bzip2 compressed archive | .tar.bz2, .tbz |
| `tar xjf archive.tar.bz2` | Extract bzip2 tar | Extract bzip2 archive | |
| `tar cJf archive.tar.xz files/` | Create xz tar | Create xz compressed archive | .tar.xz |
| `tar xJf archive.tar.xz` | Extract xz tar | Extract xz archive | |
| `gzip file` | GNU ZIP | Compress file | .gz |
| `gunzip file.gz` | GNU unZIP | Decompress gzip file | |
| `zcat file.gz` | Compressed cat | View gzipped file | |
| `bzip2 file` | Better ZIP | Compress with bzip2 | .bz2 |
| `bunzip2 file.bz2` | Extract bzip2 | Decompress bzip2 file | |
| `bzcat file.bz2` | Bzip2 cat | View bzip2 file | |
| `xz file` | XZ compress | Compress with xz | .xz |
| `unxz file.xz` | Extract xz | Decompress xz file | |
| `xzcat file.xz` | XZ cat | View xz file | |
| `zip archive.zip files/` | Create ZIP | Create zip archive | .zip |
| `zip -r archive.zip directory/` | Recursive zip | Zip entire directory | .zip |
| `unzip archive.zip` | Extract ZIP | Extract zip archive | |
| `unzip -l archive.zip` | List zip contents | List files in zip without extracting | |
| `7z a archive.7z files/` | 7-Zip add | Create 7z archive | .7z |
| `7z x archive.7z` | 7-Zip extract | Extract 7z archive | |
| `rar a archive.rar files/` | Create RAR | Create rar archive | .rar |
| `unrar x archive.rar` | Extract RAR | Extract rar archive | |

**TAR Flag Memory Tricks:**
- **c**reate, **x**tract, **f**ile, **v**erbose, **z** = gzip, **j** = bzip2, **J** = xz
- **"Create eXtracted Files Very Zealously"** 📦

---

## 1️⃣3️⃣ PACKAGE MANAGEMENT (Ubuntu/Debian) 📦

| Command | Meaning | What It Does | When to Use |
|---------|---------|--------------|-------------|
| `apt update` | Update package lists | Refresh available packages | Before installing anything |
| `apt upgrade` | Upgrade packages | Update installed packages | Regular maintenance |
| `apt full-upgrade` | Full system upgrade | Update + handle dependencies | Major updates |
| `apt install package` | Install package | Install new software | |
| `apt remove package` | Remove package | Uninstall software (keep config) | |
| `apt purge package` | Purge package | Uninstall + remove config files | Complete removal |
| `apt autoremove` | Remove unused packages | Clean up orphaned packages | Disk cleanup |
| `apt autoclean` | Clean package cache | Remove old package files | Disk cleanup |
| `apt search keyword` | Search packages | Find packages by keyword | |
| `apt show package` | Show package info | Detailed package information | |
| `apt list --installed` | List installed packages | Show all installed packages | |
| `apt list --upgradable` | List upgradable packages | Show packages that can be updated | |
| `apt-cache search keyword` | Search package cache | Alternative search method | |
| `apt-cache show package` | Show package details | Alternative to apt show | |
| `apt-cache depends package` | Show dependencies | What this package needs | |
| `apt-cache rdepends package` | Reverse dependencies | What needs this package | |
| `aptitude` | Interactive package manager | TUI package management | Alternative to apt |
| `dpkg -i package.deb` | Install .deb file | Install local .deb package | |
| `dpkg -r package` | Remove package | Remove installed package | |
| `dpkg -l` | List packages | Show all installed packages | |
| `dpkg -L package` | List package files | Show files installed by package | |
| `dpkg -S /path/to/file` | Search file owner | Which package owns this file? | |
| `snap install package` | Install snap | Install snap package | Universal packages |
| `snap list` | List snaps | Show installed snap packages | |
| `snap remove package` | Remove snap | Uninstall snap package | |
| `snap refresh` | Update snaps | Update all snap packages | |
| `flatpak install package` | Install flatpak | Install flatpak application | Sandboxed apps |
| `flatpak list` | List flatpaks | Show installed flatpak apps | |
| `add-apt-repository ppa:user/repo` | Add PPA | Add Personal Package Archive | Third-party software |

---

## 1️⃣4️⃣ SYSTEM SERVICES & INIT 🔧

| Command | Meaning | What It Does | Use Case |
|---------|---------|--------------|----------|
| `systemctl status service` | System control status | Check service status | Is nginx running? |
| `systemctl start service` | Start service | Start system service | |
| `systemctl stop service` | Stop service | Stop system service | |
| `systemctl restart service` | Restart service | Restart system service | Apply config changes |
| `systemctl reload service` | Reload service | Reload service config | Graceful config reload |
| `systemctl enable service` | Enable service | Start service at boot | |
| `systemctl disable service` | Disable service | Don't start at boot | |
| `systemctl is-active service` | Check if active | Is service currently running? | |
| `systemctl is-enabled service` | Check if enabled | Will service start at boot? | |
| `systemctl list-units` | List units | Show all systemd services | |
| `systemctl list-units --failed` | List failed units | Show failed services | Troubleshooting |
| `systemctl daemon-reload` | Reload daemon | Reload systemd configuration | After editing service files |
| `journalctl` | Journal control | View systemd logs | Modern log viewing |
| `journalctl -u service` | Logs for unit | View logs for specific service | Service troubleshooting |
| `journalctl -f` | Follow logs | Live log monitoring | Like tail -f for systemd |
| `journalctl --since "1 hour ago"` | Recent logs | Logs from last hour | |
| `journalctl --until "2023-12-31"` | Logs until date | Logs up to specific date | |
| `journalctl -p err` | Error priority logs | Only error messages | |
| `journalctl --disk-usage` | Log disk usage | How much space logs use | |
| `service servicename start` | SysV service start | Old-style service management | Legacy systems |
| `service servicename status` | SysV service status | Check SysV service | |
| `chkconfig servicename on` | Enable SysV service | Enable service at boot (old) | Legacy systems |
| `update-rc.d servicename enable` | Update run control | Enable service (Debian) | |
| `crontab -e` | Edit cron table | Edit your scheduled jobs | |
| `crontab -l` | List cron jobs | Show your scheduled jobs | |
| `crontab -u user -e` | Edit user's cron | Edit another user's cron | |
| `at now + 1 hour` | Schedule one-time task | Run command in 1 hour | |
| `atq` | AT queue | Show scheduled at jobs | |
| `atrm job_id` | Remove at job | Cancel scheduled job | |

---

## 1️⃣5️⃣ DISK & FILESYSTEM MANAGEMENT 💾

| Command | Meaning | What It Does | Important Notes |
|---------|---------|--------------|-----------------|
| `df -h` | Disk Free - Human | Show filesystem usage | Most important disk command |
| `df -i` | Disk Free - Inodes | Show inode usage | When you can't create files |
| `du -sh directory` | Disk Usage - Summary Human | Directory size | How big is this folder? |
| `du -ah directory \| sort -hr` | Sorted disk usage | Find largest files/directories | Disk cleanup |
| `fdisk -l` | List disk partitions | Show all disk partitions | Needs sudo |
| `fdisk /dev/sda` | Partition disk | Interactive disk partitioner | ⚠️ DANGEROUS! |
| `parted -l` | List partitions | Alternative to fdisk -l | |
| `lsblk` | List block devices | Show all storage devices | Tree view with mount points |
| `blkid` | Block device ID | Show device UUIDs and labels | |
| `mount` | Show mounted filesystems | Currently mounted filesystems | |
| `mount /dev/sda1 /mnt` | Mount filesystem | Mount device to directory | |
| `umount /mnt` | Unmount filesystem | Unmount filesystem | Note: no 'n' in umount! |
| `umount -f /mnt` | Force unmount | Force unmount busy filesystem | Last resort |
| `eject /dev/cdrom` | Eject removable media | Eject CD/DVD/USB | |
| `sync` | Synchronize | Force write cached data to disk | Before removing USB |
| `fsck /dev/sda1` | File System ChecK | Check and repair filesystem | ⚠️ Unmount first! |
| `fsck -f /dev/sda1` | Force filesystem check | Force check even if clean | |
| `e2fsck /dev/sda1` | Ext2/3/4 filesystem check | Check ext filesystems | |
| `mkfs.ext4 /dev/sda1` | Make ext4 filesystem | Format partition as ext4 | ⚠️ DESTROYS DATA! |
| `mkfs.ntfs /dev/sda1` | Make NTFS filesystem | Format as NTFS | For Windows compatibility |
| `mkfs.vfat /dev/sda1` | Make FAT filesystem | Format as FAT32 | For USB drives |
| `resize2fs /dev/sda1` | Resize ext filesystem | Resize ext2/3/4 filesystem | After partition resize |
| `tune2fs -l /dev/sda1` | Tune filesystem | Show filesystem information | |
| `tune2fs -c 0 /dev/sda1` | Disable fsck interval | Disable automatic fsck | |
| `dd if=/dev/zero of=/dev/sda bs=1M` | Disk dump | Write zeros to disk | ⚠️ DESTROYS EVERYTHING! |
| `dd if=/dev/sda of=backup.img bs=1M` | Create disk image | Backup entire disk | Full disk backup |
| `dd if=backup.img of=/dev/sda bs=1M` | Restore disk image | Restore from backup | ⚠️ OVERWRITES DISK! |
| `badblocks -v /dev/sda1` | Check for bad blocks | Test disk for errors | Hardware testing |
| `hdparm -I /dev/sda` | Hard disk parameters | Show disk information | |
| `hdparm -tT /dev/sda` | Disk speed test | Test disk read speed | Performance testing |
| `smartctl -a /dev/sda` | SMART info | Show disk health information | Predictive failure |

---

## 1️⃣6️⃣ MEMORY & SWAP MANAGEMENT 🧠

| Command | Meaning | What It Does | When to Use |
|---------|---------|--------------|-------------|
| `free -h` | Free memory - Human | RAM usage in GB/MB | Quick memory check |
| `free -m` | Free memory - MB | RAM usage in megabytes | |
| `free -s 2` | Free memory every 2 seconds | Continuous memory monitoring | |
| `cat /proc/meminfo` | Memory information | Detailed memory statistics | Deep memory analysis |
| `vmstat 1` | Virtual Memory Statistics | Memory/CPU stats every second | Performance monitoring |
| `vmstat -s` | Memory statistics | Memory summary | |
| `swapon -s` | Show swap | Active swap spaces | |
| `swapon /dev/sda2` | Enable swap | Activate swap partition | |
| `swapoff /dev/sda2` | Disable swap | Deactivate swap partition | |
| `mkswap /dev/sda2` | Make swap | Create swap filesystem | |
| `fallocate -l 1G /swapfile` | Create swap file | Allocate 1GB file for swap | |
| `swapon /swapfile` | Enable swap file | Activate swap file | |
| `echo 3 > /proc/sys/vm/drop_caches` | Drop caches | Free up cached memory | Needs root |
| `sync && echo 1 > /proc/sys/vm/drop_caches` | Drop page cache | Free page cache only | |

---

## 1️⃣7️⃣ ENVIRONMENT & VARIABLES 🌍

| Command | Meaning | What It Does | Scope |
|---------|---------|--------------|-------|
| `env` | Environment | Show all environment variables | |
| `printenv` | Print environment | Alternative to env | |
| `echo $HOME` | Print variable | Show HOME variable value | |
| `echo $PATH` | Print PATH | Show executable search paths | |
| `export VAR=value` | Export variable | Set environment variable | Current session |
| `unset VAR` | Unset variable | Remove environment variable | |
| `VAR=value command` | Temporary variable | Set variable for one command only | |
| `which command` | Which executable | Show path of command | |
| `type command` | Command type | How command is interpreted | |
| `alias ll='ls -la'` | Create alias | Create command shortcut | Current session |
| `unalias ll` | Remove alias | Remove command alias | |
| `alias` | List aliases | Show all current aliases | |
| `history` | Command history | Show command history | |
| `history -c` | Clear history | Clear command history | |
| `!!` | Previous command | Repeat last command | |
| `!string` | History expansion | Run last command starting with string | |
| `source ~/.bashrc` | Source file | Reload configuration file | |
| `. ~/.bashrc` | Dot source | Alternative to source | |

---

## 1️⃣8️⃣ SECURITY & PERMISSIONS 🔒

| Command | Meaning | What It Does | Security Level |
|---------|---------|--------------|----------------|
| `sudo command` | Substitute User DO | Run command as root | Temporary elevation |
| `sudo -i` | Interactive root shell | Login as root | Full root access |
| `sudo -s` | Root shell | Root shell, keep environment | |
| `sudo -u user command` | Run as user | Execute command as another user | |
| `sudo -l` | List sudo privileges | What can you sudo? | Check permissions |
| `visudo` | Edit sudoers | Safely edit sudo configuration | ⚠️ Use only this command! |
| `su` | Substitute user | Switch to root (needs root password) | |
| `su - user` | Switch user with environment | Login as another user | |
| `passwd` | Change password | Change your password | |
| `passwd user` | Change user password | Change another user's password | Needs sudo |
| `chage -l user` | Change age - list | Show password aging info | |
| `chage -M 90 user` | Set max password age | Password expires in 90 days | |
| `usermod -L user` | Lock user account | Disable user account | |
| `usermod -U user` | Unlock user account | Enable user account | |
| `last` | Last logins | Recent login history | Security auditing |
| `lastb` | Last bad logins | Failed login attempts | Security monitoring |
| `who` | Who's logged in | Current users | |
| `w` | Who and what | Users and their activities | More detailed |
| `id` | User/group IDs | Your IDs and groups | Permission debugging |
| `groups` | User groups | Groups you belong to | |
| `umask` | User mask | Default file permissions | |
| `lsattr file` | List attributes | Show file attributes | Extended file properties |
| `chattr +i file` | Change attributes - immutable | Make file unchangeable | ⚠️ Even root can't modify! |
| `chattr -i file` | Remove immutable | Make file changeable again | |
| `gpg --gen-key` | Generate GPG key | Create encryption key pair | |
| `gpg --encrypt file` | Encrypt file | Encrypt file with GPG | |
| `gpg --decrypt file.gpg` | Decrypt file | Decrypt GPG file | |

---

## 1️⃣9️⃣ NETWORK SECURITY & FIREWALL 🛡️

| Command | Meaning | What It Does | Use Case |
|---------|---------|--------------|----------|
| `ufw status` | Uncomplicated FireWall status | Show firewall status | Ubuntu firewall |
| `ufw enable` | Enable firewall | Turn on UFW firewall | |
| `ufw disable` | Disable firewall | Turn off UFW firewall | |
| `ufw allow 22` | Allow port | Allow SSH connections | |
| `ufw deny 23` | Deny port | Block telnet connections | |
| `ufw allow from 192.168.1.0/24` | Allow from subnet | Allow from specific network | |
| `ufw delete allow 22` | Delete rule | Remove firewall rule | |
| `iptables -L` | List iptables rules | Show firewall rules | Low-level firewall |
| `iptables -A INPUT -p tcp --dport 22 -j ACCEPT` | Add iptables rule | Allow SSH with iptables | |
| `iptables-save` | Save iptables | Backup current rules | |
| `iptables-restore` | Restore iptables | Restore saved rules | |
| `fail2ban-client status` | Fail2ban status | Show intrusion detection status | |
| `fail2ban-client status sshd` | SSH jail status | Show SSH protection status | |
| `nmap localhost` | Network mapper | Scan local ports | Port scanning |
| `nmap -sS target` | SYN scan | Stealth port scan | Security testing |
| `netstat -tulpn` | Network connections | Show listening services | |
| `ss -tulpn` | Socket statistics | Modern netstat | |
| `lsof -i` | List open files - internet | Network connections by process | |
| `tcpdump -i eth0` | Capture packets | Network traffic analysis | |
| `arp -a` | ARP table | IP to MAC address mappings | |

---

## 2️⃣0️⃣ ADVANCED SYSTEM ANALYSIS 🔬

| Command | Meaning | What It Does | Expert Level |
|---------|---------|--------------|--------------|
| `strace command` | System call trace | Trace system calls | Debug programs |
| `ltrace command` | Library trace | Trace library calls | Debug shared libraries |
| `ldd /bin/ls` | List dynamic dependencies | Show shared library dependencies | |
| `objdump -d binary` | Object dump - disassemble | Disassemble binary | Assembly analysis |
| `readelf -h binary` | Read ELF header | Show binary format info | |
| `file /bin/ls` | File type | Identify file type | |
| `stat file` | File statistics | Detailed file information | |
| `inotifywait -m /path` | Watch file changes | Monitor file modifications | |
| `perf top` | Performance top | CPU profiling | Performance analysis |
| `perf record command` | Record performance | Profile command execution | |
| `valgrind command` | Memory debugger | Find memory leaks | Debug C/C++ programs |
| `gdb program` | GNU Debugger | Debug programs | |
| `core_pattern` | Core dump pattern | Configure crash dumps | |
| `ulimit -a` | User limits | Show resource limits | |
| `ulimit -c unlimited` | Set core dump size | Allow core dumps | |
| `/proc/cpuinfo` | CPU information | Processor details | |
| `/proc/meminfo` | Memory information | RAM details | |
| `/proc/version` | Kernel version | Kernel information | |
| `/proc/cmdline` | Kernel command line | Boot parameters | |
| `/proc/mounts` | Mounted filesystems | Currently mounted filesystems | |
| `/proc/net/dev` | Network device stats | Network interface statistics | |

---

## 2️⃣1️⃣ TEXT PROCESSING POWERHOUSE 📝

| Command | Meaning | What It Does | Power Usage |
|---------|---------|--------------|-------------|
| `awk '{print $1}' file` | Print first field | Extract first column | |
| `awk -F: '{print $1}' /etc/passwd` | Field separator | Use : as separator | |
| `awk '$3 > 100' file` | Conditional processing | Print lines where 3rd field > 100 | |
| `awk '{sum+=$1} END {print sum}' file` | Sum calculation | Add up first column | |
| `sed 's/old/new/g' file` | Global substitution | Replace all occurrences | |
| `sed -n '10,20p' file` | Print lines 10-20 | Extract specific line range | |
| `sed '/pattern/d' file` | Delete matching lines | Remove lines containing pattern | |
| `sed -i.bak 's/old/new/g' file` | In-place edit with backup | Modify file, keep backup | |
| `tr 'a-z' 'A-Z' < file` | Translate characters | Convert to uppercase | |
| `tr -d '\r' < file` | Delete characters | Remove carriage returns | Windows to Unix |
| `tr -s ' ' < file` | Squeeze characters | Collapse multiple spaces to one | |
| `cut -d: -f1,3 /etc/passwd` | Cut specific fields | Extract username and UID | |
| `cut -c1-10 file` | Cut characters | Extract character positions 1-10 | |
| `paste file1 file2` | Paste files | Merge files side by side | |
| `join file1 file2` | Join files | Merge files on common field | |
| `comm file1 file2` | Compare files | Show unique and common lines | Files must be sorted |
| `diff file1 file2` | Difference | Show differences between files | |
| `diff -u file1 file2` | Unified diff | Better diff format | |
| `patch < file.diff` | Apply patch | Apply diff to files | |
| `column -t file` | Columnize | Format into columns | |
| `expand file` | Expand tabs | Convert tabs to spaces | |
| `unexpand file` | Convert spaces to tabs | Opposite of expand | |
| `fmt file` | Format text | Reformat paragraph | |
| `fold -w 80 file` | Fold lines | Wrap lines at 80 characters | |
| `nl file` | Number lines | Add line numbers | |
| `pr file` | Print formatted | Format for printing | |
| `split -l 1000 file` | Split file | Split into 1000-line chunks | |
| `csplit file '/pattern/'` | Context split | Split file at pattern matches | |

---

## 2️⃣2️⃣ PROCESS SCHEDULING & PRIORITIES ⚖️

| Command | Meaning | What It Does | Priority Range |
|---------|---------|--------------|----------------|
| `nice -n 10 command` | Run with nice value | Lower priority process | -20 (highest) to 19 (lowest) |
| `renice 5 PID` | Change nice value | Adjust running process priority | |
| `ionice -c 3 command` | I/O nice | Lower I/O priority | Classes 1-3 |
| `ionice -p PID` | Check I/O priority | Show process I/O priority | |
| `taskset -c 0,1 command` | Set CPU affinity | Run on specific CPU cores | |
| `taskset -p PID` | Show CPU affinity | Which cores process uses | |
| `chrt -r 50 command` | Change real-time priority | Set real-time scheduling | Real-time priorities |
| `chrt -p PID` | Show scheduling policy | Display scheduling info | |
| `cpulimit -l 50 command` | Limit CPU usage | Restrict to 50% CPU | Percentage limit |
| `timeout 30s command` | Timeout command | Kill command after 30 seconds | |
| `cgroups` | Control groups | Resource limitation system | Advanced resource control |

---

## 2️⃣3️⃣ KERNEL & MODULE MANAGEMENT 🧠

| Command | Meaning | What It Does | Use Case |
|---------|---------|--------------|----------|
| `lsmod` | List modules | Show loaded kernel modules | |
| `modinfo module` | Module information | Show module details | |
| `insmod module.ko` | Insert module | Load kernel module | |
| `rmmod module` | Remove module | Unload kernel module | |
| `modprobe module` | Probe module | Smart module loading | Handles dependencies |
| `modprobe -r module` | Remove module safely | Unload with dependency checking | |
| `depmod` | Dependency modules | Update module dependencies | After installing modules |
| `mkinitrd` | Make initial ramdisk | Create boot ramdisk | Boot process |
| `dracut` | Dynamic root | Modern initramfs creator | |
| `update-initramfs -u` | Update initramfs | Rebuild boot image | Ubuntu/Debian |
| `dkms status` | Dynamic Kernel Module Support | Show DKMS modules | |
| `sysctl -a` | System control | Show all kernel parameters | |
| `sysctl vm.swappiness=10` | Set kernel parameter | Adjust swappiness | |
| `sysctl -p` | Load sysctl config | Apply /etc/sysctl.conf | |
| `cat /proc/version` | Kernel version | Show kernel info | |
| `uname -r` | Kernel release | Just kernel version | |

---

## 2️⃣4️⃣ BACKUP & RECOVERY 💾

| Command | Meaning | What It Does | Best Practice |
|---------|---------|--------------|---------------|
| `rsync -av source/ backup/` | Sync backup | Incremental backup | Most efficient |
| `rsync --link-dest=../yesterday today/ backup/` | Hard link backup | Space-efficient snapshots | |
| `tar czf backup.tar.gz /home/` | Tar backup | Compressed archive backup | |
| `dd if=/dev/sda of=disk.img bs=1M` | Disk image | Bit-for-bit disk copy | Complete disk backup |
| `ddrescue /dev/sda backup.img` | Data recovery | Recover from damaged disks | Better than dd for recovery |
| `fsarchiver savefs backup.fsa /dev/sda1` | Filesystem backup | Backup just filesystem | |
| `clonezilla` | Disk cloning tool | GUI disk imaging | |
| `duplicity /home file:///backup` | Encrypted backup | Encrypted incremental backups | |
| `rdiff-backup /home /backup` | Reverse incremental | Space-efficient backups | |
| `borgbackup create /backup::today /home` | Deduplicating backup | Advanced backup tool | |
| `rclone sync /home remote:backup` | Cloud sync | Backup to cloud storage | |
| `restic backup /home` | Modern backup | Fast, secure backups | |
| `testdisk` | Data recovery | Recover lost partitions | |
| `photorec` | Photo recovery | Recover deleted files | |
| `extundelete /dev/sda1` | Ext undelete | Recover deleted files from ext | |

---

## 🎯 COMMAND COMBINATION EXAMPLES

### 🔥 Power Combos That'll Make You Look Like a Linux Wizard

```bash
# Find and kill all processes by name
ps aux | grep firefox | grep -v grep | awk '{print $2}' | xargs kill

# Find largest files in directory
find /path -type f -exec du -h {} + | sort -hr | head -20

# Monitor log file with colored output
tail -f /var/log/syslog | grep --color=always ERROR

# Find files modified in last 24 hours
find /home -type f -mtime -1 -ls

# Backup with progress bar
rsync -av --progress source/ dest/

# Find duplicate files
find /path -type f -exec md5sum {} + | sort | uniq -d -w 32

# Network connections by process
netstat -tulpn | grep :80

# System info one-liner
echo "$(hostname) - $(uptime) - $(df -h / | awk 'NR==2{print $4" free"}')"

# Find SUID files (security audit)
find / -perm -4000 -type f 2>/dev/null

# Memory usage by process
ps aux --sort=-%mem | head -10

# Disk usage by directory, sorted
du -h /var/* 2>/dev/null | sort -hr | head -10

# Network bandwidth usage
iftop -t -s 10

# Find world-writable files
find / -type f -perm -002 2>/dev/null

# System resource summary
echo "CPU: $(nproc) cores, RAM: $(free -h | awk '/^Mem/{print $2}'), Disk: $(df -h / | awk 'NR==2{print $2}')"
```

---

## 🏆 ULTIMATE PRO TIPS & BEST PRACTICES
### 🚀 Efficiency Hacks
- **Tab completion is life** - Don't type full paths!
- **Use `!!` to repeat last command** - Especially with `sudo !!`
- **Ctrl+R for reverse search** - Find commands from history
- **Use `cd -` to toggle between directories** - Like browser back button
- **Combine commands with `&&`, `||`, and `;`** - Chain operations
- **Use `nohup command &` for long-running tasks** - Survives logout
- **Learn regex** - `grep`, `sed`, `awk` become superpowers

### ⚠️ Safety First
- **Always double-check `rm` commands** - No recycle bin in Linux!
- **Use `rm -i` for interactive deletion** - Asks before deleting
- **Test complex commands on copies first** - Murphy's Law loves Linux
- **Keep backups** - rsync is your friend
- **Use `--dry-run` with rsync** - Preview what will happen
- **Quote variables in scripts** - `"$variable"` prevents word splitting
- **Use full paths in scripts** - `/bin/ls` instead of just `ls`

### 🎯 Problem-Solving Approach
1. **Check logs first** - `journalctl`, `/var/log/`, `dmesg`
2. **Use `man command`** - RTFMs are actually helpful
3. **Check if service is running** - `systemctl status service`
4. **Verify permissions** - `ls -la`, `id`, `groups`
5. **Test network connectivity** - `ping`, `nslookup`, `netstat`
6. **Monitor resources** - `top`, `htop`, `df`, `free`
7. **Google the error message** - You're not the first to see it!

### 🧠 Mental Models
- **Everything is a file** - Devices, processes, network connections
- **Pipes connect programs** - Output of one becomes input of another
- **Permissions are hierarchical** - User → Group → Other
- **Environment variables are inherited** - Export to pass to child processes
- **Signals control processes** - TERM (15) asks nicely, KILL (9) doesn't
- **Exit codes matter** - 0 = success, anything else = error

---

## 📚 QUICK REFERENCE SECTIONS

### File Permission Numbers
```
7 = rwx (4+2+1)    6 = rw- (4+2+0)    5 = r-x (4+0+1)    4 = r-- (4+0+0)
3 = -wx (0+2+1)    2 = -w- (0+2+0)    1 = --x (0+0+1)    0 = --- (0+0+0)
```

### Signal Numbers
```
1 = HUP (hangup)      2 = INT (interrupt/Ctrl+C)    3 = QUIT
9 = KILL (force kill) 15 = TERM (terminate nicely)  18 = CONT (continue)
19 = STOP (pause)     20 = TSTP (Ctrl+Z)
```

### Log Locations
```
/var/log/syslog       - General system messages
/var/log/auth.log     - Authentication logs
/var/log/kern.log     - Kernel messages
/var/log/apache2/     - Apache web server
/var/log/nginx/       - Nginx web server
/var/log/mysql/       - MySQL database
/var/log/postgresql/  - PostgreSQL database
journalctl -f         - Follow systemd logs live
```

### Essential Keyboard Shortcuts
```
Ctrl+C    - Kill current process
Ctrl+Z    - Suspend process (use 'fg' to resume)
Ctrl+D    - End of input / logout
Ctrl+L    - Clear screen
Ctrl+A    - Move to beginning of line
Ctrl+E    - Move to end of line
Ctrl+U    - Delete from cursor to beginning
Ctrl+K    - Delete from cursor to end
Alt+.     - Insert last argument from previous command
```

### Network Commands Cheat Sheet
```
ss -tuln              - Show listening ports (modern netstat)
ip addr show          - Show network interfaces
ip route show         - Show routing table
curl -I website.com   - Check HTTP headers
wget -O- url          - Download to stdout
nc -zv host port      - Test if port is open
```

### Process Management
```
ps aux | grep process     - Find running processes
pgrep process_name        - Get PID by name
pkill process_name        - Kill by name
jobs                      - Show background jobs
fg %1                     - Bring job 1 to foreground
bg %1                     - Send job 1 to background
nohup command &           - Run detached from terminal
```

### File Operations Power Moves
```
find . -name "*.txt" -exec rm {} \;     - Find and delete
find . -type f -mtime +30 -delete       - Delete files older than 30 days
tar -czf backup.tar.gz /path/to/dir     - Create compressed archive
tar -xzf backup.tar.gz                  - Extract compressed archive
rsync -av --progress src/ dest/         - Sync with progress
```

### Text Processing Wizardry
```
grep -r "pattern" .                     - Recursive search
grep -n "pattern" file                  - Show line numbers
sed 's/old/new/g' file                  - Replace all occurrences
awk '{print $1}' file                   - Print first column
sort file | uniq -c                     - Count unique lines
head -n 20 file                         - First 20 lines
tail -f /var/log/syslog                 - Follow log file
```

### System Information Commands
```
uname -a              - System information
lsb_release -a        - Ubuntu version info
df -h                 - Disk space usage
free -h               - Memory usage
lscpu                 - CPU information
lsblk                 - Block devices
lsusb                 - USB devices
lspci                 - PCI devices
```

---

## 🛠️ ESSENTIAL UBUNTU COMMANDS FOR DAILY USE

### Package Management Mastery
```bash
# Update package list
sudo apt update

# Upgrade all packages
sudo apt upgrade

# Install package
sudo apt install package_name

# Remove package (keep config files)
sudo apt remove package_name

# Remove package and config files
sudo apt purge package_name

# Remove orphaned packages
sudo apt autoremove

# Search for packages
apt search keyword

# Show package information
apt show package_name

# List installed packages
apt list --installed

# Fix broken packages
sudo apt --fix-broken install
```

### Service Management (systemctl)
```bash
# Start/stop/restart service
sudo systemctl start service_name
sudo systemctl stop service_name
sudo systemctl restart service_name

# Enable/disable service at boot
sudo systemctl enable service_name
sudo systemctl disable service_name

# Check service status
systemctl status service_name

# List all services
systemctl list-units --type=service

# Reload systemd configuration
sudo systemctl daemon-reload
```

### User and Permission Management
```bash
# Add new user
sudo adduser username

# Add user to group
sudo usermod -aG groupname username

# Change file ownership
sudo chown user:group filename

# Change permissions recursively
sudo chmod -R 755 /path/to/directory

# View current user's groups
groups

# Switch to another user
su - username

# Run command as another user
sudo -u username command
```

---

## 🔧 TROUBLESHOOTING COMMON ISSUES

### When Things Go Wrong
```bash
# Check disk space (most common issue)
df -h

# Check memory usage
free -h

# Check running processes
htop

# Check system logs
journalctl -xe

# Check specific service logs
journalctl -u service_name -f

# Check boot messages
dmesg | less

# Test network connectivity
ping google.com
nslookup google.com
```

### Recovery Commands
```bash
# Boot into recovery mode (select from GRUB menu)
# Then run:
fsck /dev/sdaX        # Check filesystem
mount -o remount,rw / # Remount root as writable

# Reset user password (as root)
passwd username

# Fix GRUB bootloader
sudo update-grub
sudo grub-install /dev/sda
```

---

## 🎓 GRADUATION CHECKLIST

You're ready to rock Linux when you can:
- [ ] Navigate the filesystem blindfolded with `cd`, `ls`, `pwd`
- [ ] Create, edit, and manage files with `nano`, `vim`, `touch`, `cp`, `mv`, `rm`
- [ ] Understand and set file permissions with `chmod`, `chown`
- [ ] Install and manage software with `apt`
- [ ] Monitor system resources with `top`, `df`, `free`
- [ ] Search and manipulate text with `grep`, `sed`, `awk`
- [ ] Manage processes with `ps`, `kill`, `jobs`
- [ ] Work with archives using `tar`, `zip`
- [ ] Use pipes and redirection like a pro
- [ ] Write basic shell scripts
- [ ] Troubleshoot common issues using logs
- [ ] Configure and manage services with `systemctl`

---

## 🚀 FINAL WORDS

Remember: Linux mastery isn't about memorizing every command – it's about understanding the patterns and having the confidence to experiment. The terminal is your friend, not your enemy. Every expert was once a beginner who kept practicing.

**Most important rule**: When in doubt, `man command` it out!

Now go forth and `sudo` like a boss! 🐧✨
