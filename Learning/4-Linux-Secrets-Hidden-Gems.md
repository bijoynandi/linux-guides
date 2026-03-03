# Linux Secrets & Hidden Gems Cheat Sheet 🐧

*For the curious Ubuntu explorer who wants to unlock Linux's superpowers!*

## 🖱️ Copy/Paste Magic (You just discovered this!)
- **Select text** = Automatically copied to primary clipboard
- **Middle-click** = Paste from primary selection
- **Double-click** = Select whole word
- **Triple-click** = Select whole line
- Works everywhere: terminal, browser, text editor!

## ⚡ Terminal Superpowers

### Navigation Shortcuts
- `Ctrl + A` = Jump to beginning of line
- `Ctrl + E` = Jump to end of line
- `Ctrl + U` = Delete everything before cursor
- `Ctrl + K` = Delete everything after cursor
- `Ctrl + W` = Delete word before cursor
- `Alt + F` = Jump forward one word
- `Alt + B` = Jump backward one word

### History Magic
- `!!` = Run the last command again
- `!ssh` = Run the last command that started with "ssh"
- `Ctrl + R` = Search through command history (type to search, Enter to run)
- `history | grep keyword` = Find specific commands you ran before

### Directory Shortcuts
- `cd -` = Go back to previous directory (like browser back button!)
- `pushd /path` = Go to path but remember current location
- `popd` = Return to the location you "pushed" from
- `dirs` = See your directory stack

## 🔍 File & Text Wizardry

### Quick File Operations
- `ls -la` = List with details, including hidden files
- `ls -lh` = List with human-readable file sizes
- `ls -lt` = List sorted by modification time (newest first)
- `find . -name "*.txt"` = Find all .txt files in current directory and subdirs
- `locate filename` = Find files by name (super fast!)
- `which command` = Show where a command is located

### Text Superpowers
- `grep -r "search term" .` = Search for text in all files recursively
- `grep -i "search"` = Case-insensitive search
- `tail -f filename` = Watch file for changes in real-time (great for logs!)
- `head -20 filename` = Show first 20 lines
- `wc -l filename` = Count lines in file
- `sort filename | uniq` = Remove duplicate lines

## 🚀 Process & System Magic

### Process Control
- `htop` = Better version of `top` (install with `sudo apt install htop`)
- `ps aux | grep process_name` = Find specific running processes
- `killall process_name` = Kill all processes with that name
- `nohup command &` = Run command that survives terminal close
- `jobs` = See background jobs
- `fg` = Bring background job to foreground

### System Info
- `df -h` = Show disk usage in human-readable format
- `free -h` = Show RAM usage
- `lscpu` = Show CPU information
- `lsusb` = List USB devices
- `lspci` = List PCI devices
- `uname -a` = Show system information

## 🎨 Customization Secrets

### Bash Prompt Magic
Add to `~/.bashrc`:
```bash
# Colorful prompt with git branch
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
```

### Useful Aliases
Add to `~/.bashrc`:
```bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias h='history'
alias c='clear'
alias update='sudo apt update && sudo apt upgrade'
```

## 🔧 Power User Tricks

### File Permissions Made Easy
- `chmod +x filename` = Make file executable
- `chmod 755 filename` = rwxr-xr-x (owner: read/write/execute, others: read/execute)
- `chmod 644 filename` = rw-r--r-- (owner: read/write, others: read only)

### Archive & Compression
- `tar -czf backup.tar.gz folder/` = Create compressed archive
- `tar -xzf backup.tar.gz` = Extract compressed archive
- `zip -r backup.zip folder/` = Create zip file
- `unzip backup.zip` = Extract zip file

### Network Shortcuts
- `curl wttr.in/Kolkata` = Get weather in terminal!
- `curl ifconfig.me` = Get your public IP address
- `ping -c 4 google.com` = Ping 4 times then stop
- `wget -c url` = Resume interrupted download

## 🎯 Keyboard Shortcuts for Terminal

### Essential Shortcuts
- `Ctrl + C` = Kill current process
- `Ctrl + Z` = Suspend current process (use `fg` to resume)
- `Ctrl + D` = Exit terminal/logout
- `Ctrl + L` = Clear screen (same as `clear`)
- `Tab` = Auto-complete commands/filenames
- `Tab Tab` = Show all possible completions

### Advanced Navigation
- `Ctrl + Left/Right Arrow` = Jump between words
- `Home/End` = Jump to beginning/end of line
- `Ctrl + Shift + C/V` = Copy/paste in terminal (you know this pain!)
- `Ctrl + Shift + T` = Open new terminal tab
- `Ctrl + Shift + W` = Close current terminal tab

## 🕵️ Secret Commands That Will Blow Your Mind

### Fun & Useful
- `tree` = Show directory structure as a tree (install: `sudo apt install tree`)
- `ncdu` = Interactive disk usage analyzer (install: `sudo apt install ncdu`)
- `htop` = Beautiful process monitor (install: `sudo apt install htop`)
- `tldr command_name` = Get simple examples of commands (install: `sudo apt install tldr`)

### System Monitoring
- `watch -n 1 'df -h'` = Update disk usage every second
- `iostat` = Monitor disk I/O (install: `sudo apt install sysstat`)
- `nethogs` = See which processes are using network (install: `sudo apt install nethogs`)

### Quick Calculations
- `bc` = Calculator in terminal (try: `echo "scale=2; 22/7" | bc`)
- `date -d "2 weeks ago"` = Date calculations
- `cal` = Show calendar
- `factor 1234` = Find prime factors of a number

## 🎪 The Really Cool Stuff

### Multiple Commands
- `command1 && command2` = Run command2 only if command1 succeeds
- `command1 || command2` = Run command2 only if command1 fails
- `command1; command2` = Run both commands regardless
- `command1 | command2` = Pipe output of command1 to command2

### Redirection Magic
- `command > file` = Save output to file (overwrite)
- `command >> file` = Append output to file
- `command 2> file` = Save errors to file
- `command &> file` = Save both output and errors to file

### Background Processing
- `command &` = Run command in background
- `nohup command &` = Run command that survives terminal closure
- `screen` or `tmux` = Create persistent terminal sessions

## 🏆 Pro Tips

1. **Use Tab completion religiously** - it prevents typos and saves time
2. **Learn one new command each day** - you'll be a wizard in no time
3. **Read man pages**: `man command_name` for detailed help
4. **Use `apropos keyword`** to find commands related to what you want to do
5. **Practice in a VM first** before trying risky commands on your main system

## 🎉 Ubuntu-Specific Goodies

- `apt list --upgradable` = See what packages can be updated
- `apt search keyword` = Search for packages
- `snap list` = List installed snap packages
- `ubuntu-drivers devices` = See available drivers
- `do-release-upgrade` = Upgrade to newer Ubuntu version

---

*Remember: With great power comes great responsibility. Always backup important data before experimenting!*

**The roadside dhaba dinner is on you once you master these! 🍛**
