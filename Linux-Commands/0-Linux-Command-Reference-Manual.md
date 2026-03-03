# The Complete Unix/Linux Command Reference Manual 🐧
*Your Journey from Confusion to Unix Mastery*

> *"In this environment you get the maximum possible flexibility... so you don't have anything hard coded that will be applied everywhere."* - The wisdom discovered during our journey! 🎯

---

## Table of Contents 📋
1. [The Holy Trinity: Command Structure](#the-holy-trinity-command-structure-)
2. [The Great Quote Wars: Single vs Double](#the-great-quote-wars-single-vs-double-)
3. [Environment Variables: The System's Secret Language](#environment-variables-the-systems-secret-language-)
4. [Command Substitution: Commands Within Commands](#command-substitution-commands-within-commands-)
5. [Redirection: Controlling the Data Flow](#redirection-controlling-the-data-flow-)
6. [Pipes: The Unix Philosophy in Action](#pipes-the-unix-philosophy-in-action-)
7. [Regular Expressions: The Funny Members](#regular-expressions-the-funny-members-)
8. [Advanced Combinations: Unix Wizardry](#advanced-combinations-unix-wizardry-)
9. [File Permissions: Who Can Do What](#file-permissions-who-can-do-what-)
10. [Process Management: Controlling Your Digital Kingdom](#process-management-controlling-your-digital-kingdom-)
11. [Text Processing Tools: The Power Trio](#text-processing-tools-the-power-trio-)

---

## The Holy Trinity: Command Structure 👑

Every Unix command follows this sacred pattern:
```bash
COMMAND [OPTIONS] [ARGUMENTS]
```

### 🎯 The Parts Explained

**COMMAND** 📟
- The program you want to run
- Examples: `ls`, `cat`, `grep`, `mkdir`

**OPTIONS (FLAGS)** 🚩
- Modify how the command behaves
- **Short options**: `-l`, `-a`, `-h` (single dash + letter)
- **Long options**: `--long`, `--all`, `--help` (double dash + word)

**ARGUMENTS** 📝
- What the command acts on (files, directories, text)

### 🌟 Real Examples That Make Sense

```bash
ls                    # Just the command
ls -l                 # Command + option
ls -l /home          # Command + option + argument
ls -lah /home        # Command + combined options + argument
mkdir -p projects/web/html  # Command + option + argument
```

### 🤝 Combining Short Options
```bash
ls -l -a -h          # Separate (works)
ls -lah              # Combined (preferred)
ls -l -ah            # Mixed (also works)
```

> **Remember**: Long options can't be combined! `--long --all` ✅, `--longall` ❌

### 🔍 Finding Help (Your Best Friends)
```bash
command --help       # Quick reference
man command         # Full manual (press 'q' to quit)
info command        # Even more detailed (Tab to navigate)
```

---

## The Great Quote Wars: Single vs Double 🥊

*"The key thing is being flexible!"* - Our discovered wisdom

### 💪 Single Quotes: The Literal Warriors
```bash
echo 'Hello $USER, today is $(date)'
# Output: Hello $USER, today is $(date)
```
- **Everything inside stays exactly as written**
- No variable expansion, no command substitution
- Perfect for literal strings and shell code

### 🎭 Double Quotes: The Interpreters
```bash
echo "Hello $USER, today is $(date)"
# Output: Hello bijoy, today is Sun Aug 17 14:30:22 PDT 2025
```
- **Variables get expanded**: `$USER` becomes `bijoy`
- **Commands get substituted**: `$(date)` becomes actual date
- **Escape sequences work**: `\n`, `\t`

### 🎯 When to Use Which

**Single quotes for:**
- Literal text: `echo 'Use $HOME to access home directory'`
- Shell scripts: `echo 'export PATH=$PATH:/new/path' >> ~/.bashrc`
- When you want to prevent expansion

**Double quotes for:**
- Variable expansion: `echo "Welcome $USER"`
- Command substitution: `echo "Today is $(date)"`
- File paths with spaces: `cp "$file_with_spaces" /tmp/`

### 🧠 The Clever Trick: Opposite Quotes
```bash
echo 'He said "Hello World"'     # No escaping needed!
echo "Don't forget to save"      # No escaping needed!
```

> **Pro Tip**: The muscle memory you build with single quotes pays off in SQL! 💎

---

## Environment Variables: The System's Secret Language 🗣️

Think of environment variables as the system's **named storage containers**.

### 🔍 Viewing Variables
```bash
echo $HOME           # Your home directory
echo $USER           # Your username
echo $PATH           # Where shell looks for commands
echo $BROWSER        # Your default browser (if set)
env                  # Show all variables
env | grep BROWSER   # Check if BROWSER exists
```

### 📦 Setting Variables

**For current session only:**
```bash
MY_NAME="Bijoy"
echo $MY_NAME        # Shows: Bijoy
```

**For child processes (export):**
```bash
export MY_NAME="Bijoy"
# Now available to programs you run in this session
```

**Make it permanent:**
```bash
echo 'export BROWSER="firefox"' >> ~/.bashrc
source ~/.bashrc     # Reload config
```

### 🎯 The Key Difference: Regular vs Exported

```bash
# Test this yourself!
TEST_VAR="hello"
export EXPORTED_VAR="world"

bash                 # Start new shell (child process)
echo $TEST_VAR       # Shows nothing! 😱
echo $EXPORTED_VAR   # Shows "world" 😊
exit                 # Back to parent shell
```

> **Memory Hook**: `export` doesn't make it permanent - it makes it available to **child processes**!

### 🛣️ The Mighty $PATH Variable
```bash
echo $PATH
# Shows: /usr/local/bin:/usr/bin:/bin:/home/bijoy/.local/bin
```

When you type `ls`, shell searches these directories in order:
1. `/usr/local/bin/ls` (not found)
2. `/usr/bin/ls` (found! 🎉)

---

## Command Substitution: Commands Within Commands 🎪

**Not variables** - these are **commands being executed**!

### 🚀 Modern Syntax (Preferred)
```bash
echo "Today is $(date)"
echo "I am in $(pwd) directory"
echo "Current user: $(whoami)"
```

### 🦕 Old Syntax (Still Works)
```bash
echo "Today is `date`"
echo "I am in `pwd` directory"
```

### 🏗️ Practical Magic
```bash
# Store command output
CURRENT_DIR=$(pwd)
USER_COUNT=$(who | wc -l)

# Create timestamped backups
cp important.txt important_backup_$(date +%Y%m%d).txt

# Nested substitution (commands within commands within commands!)
echo "Files in home: $(ls $(echo $HOME) | wc -l)"
```

### 🎯 Commands vs Variables Quick Reference
```bash
# VARIABLES (stored values)
echo $HOME           # Shows: /home/bijoy
echo $USER           # Shows: bijoy

# COMMAND SUBSTITUTION (executes programs)
echo $(pwd)          # RUNS pwd command
echo $(whoami)       # RUNS whoami command
```

---

## Redirection: Controlling the Data Flow 🌊

Every command has three streams (like plumbing! 🚰):
- **stdin (0)**: Standard Input (keyboard by default)
- **stdout (1)**: Standard Output (screen by default)
- **stderr (2)**: Standard Error (screen for errors)

### 📤 Output Redirection
```bash
ls > files.txt               # Save output to file (overwrite)
ls >> files.txt              # Append output to file
date > today.txt             # Save today's date

# The "funny" error redirection
ls /nonexistent 2> errors.txt        # Save errors to file
ls /nonexistent 2>> errors.txt       # Append errors to file

# Both output and errors
ls /home /fake > output.txt 2> errors.txt    # Separate files
ls /home /fake > all.txt 2>&1               # Both to same file
ls /home /fake &> all.txt                   # Shorthand for above
```

### 📥 Input Redirection
```bash
cat < file.txt               # Read from file instead of keyboard
sort < names.txt             # Sort file contents (displays on screen)
wc -l < file.txt            # Count lines (shows just the number)
```

### 🗑️ The Magical /dev/null (The Black Hole)
```bash
ls > /dev/null              # Send output to oblivion
ls 2> /dev/null             # Send errors to oblivion  
ls &> /dev/null             # Send everything to oblivion (silent!)
```

### 🤔 Decoding the "Weird" Symbols
- `2>` = "redirect error stream to..."
- `2>&1` = "send errors to wherever normal output is going"
- `&>` = "send both output and errors to..."
- `>>` = "append to file" vs `>` = "overwrite file"

---

## Pipes: The Unix Philosophy in Action 🔗

*"Do one thing well, and work together"* - The Unix Way

### 🚰 Basic Pipe Concept
```bash
command1 | command2
# Output of command1 becomes input of command2
```

### 🎯 Simple Examples
```bash
ls | wc -l                   # Count files in directory
ls | grep ".txt"             # Show only .txt files
ls -la | head -5             # Show first 5 files
ps aux | grep firefox        # Find Firefox processes
```

### ⚡ Power Combinations
```bash
# Find largest files
ls -la | sort -k5 -nr | head -10

# Count file types
ls | grep -o '\.[^.]*$' | sort | uniq -c

# Analyze log files
cat access.log | grep "ERROR" | wc -l
cat access.log | cut -d' ' -f1 | sort | uniq -c | sort -nr
```

### 🔗 Chaining Multiple Pipes
```bash
ls -la | grep ".txt" | sort | head -3
# 1. List files in detail
# 2. Filter for .txt files
# 3. Sort alphabetically  
# 4. Show first 3
```

> **The Beauty**: Each command does one job perfectly, but together they're unstoppable! 💪

---

## Regular Expressions: The Funny Members 🎭

*"What are these funny members that are peeking grep?"* - Your memorable question! 😄

### 🎯 Decoding `'\.[^.]*$'` Step by Step

```bash
'\.[^.]*$'
```

- `\.` = Literal dot (escaped because `.` means "any character")
- `[^.]` = Any character that is NOT a dot
- `*` = Zero or more of the previous
- `$` = End of line

**Translation**: "Match a dot, followed by any non-dot characters until the end"
**Purpose**: Extracts file extensions! 📁

### 🌟 Common Regex Patterns
```bash
grep 'hello' file.txt        # Contains "hello"
grep '^hello' file.txt       # Starts with "hello"
grep 'hello$' file.txt       # Ends with "hello"
grep '[0-9]' file.txt        # Contains any digit
grep '[A-Z]' file.txt        # Contains any uppercase letter
```

### 🔍 Real-World Usage
```bash
# Extract file extensions and count them
ls | grep -o '\.[^.]*$' | sort | uniq -c

# Find all email addresses
grep -o '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file.txt
```

---

## Advanced Combinations: Unix Wizardry 🧙‍♂️

### 🔍 File Analysis Magic
```bash
# Find and count files
find . -name "*.txt" | wc -l
find . -type f | wc -l              # Count all files recursively

# Process found files
find . -name "*.txt" | xargs wc -l   # Count lines in all .txt files
```

### 📊 System Analysis Spells
```bash
# Top memory users
ps aux | sort -k4 -nr | head -10

# Disk usage leaders
du -h | sort -hr | head -20

# Count non-root processes
ps aux | grep -v "root" | wc -l
```

### 📈 CSV Data Analysis
```bash
# Extract and analyze columns
cat data.csv | cut -d',' -f2 | tail -n +2 | sort | uniq -c

# Count unique values in second column (skip header)
```

---

## File Permissions: Who Can Do What 🔐

### 🎭 The Permission Players
- **Owner (u)**: The file's creator
- **Group (g)**: Users in the same group  
- **Others (o)**: Everyone else

### 📖 Reading Permission Notation
```bash
ls -la
# -rwxr-xr-- 1 bijoy users 1024 Aug 17 10:30 script.sh
#  ||||||||| 
#  ||||||||└─ Others permissions
#  ||||||└─── Group permissions  
#  ||||└───── Owner permissions
#  |||└────── Special bits
#  ||└─────── Number of links
#  |└──────── File type (- = file, d = directory)
#  └───────── File type indicator
```

### 🔧 Permission Types
- **r (4)**: Read - can view file contents
- **w (2)**: Write - can modify file
- **x (1)**: Execute - can run as program

### 🛠️ Changing Permissions
```bash
chmod 755 script.sh          # rwxr-xr-x (owner: all, others: read+execute)
chmod u+x script.sh          # Add execute for owner
chmod g-w script.sh          # Remove write for group  
chmod a+r file.txt           # Add read for all (a = all)
```

### 👑 Changing Ownership
```bash
chown bijoy:users file.txt   # Change owner and group
chown bijoy file.txt         # Change owner only
chgrp users file.txt         # Change group only
```

### 🎯 Common Permission Patterns
```bash
644  # rw-r--r-- (files: owner read/write, others read)
755  # rwxr-xr-x (executables: owner all, others read/execute)  
700  # rwx------ (private: owner only)
666  # rw-rw-rw- (everyone can read/write)
```

---

## Process Management: Controlling Your Digital Kingdom 👑

### 👁️ Viewing Processes
```bash
ps                   # Your processes
ps aux              # All processes (detailed)
ps aux | grep firefox   # Find specific process
top                 # Real-time process monitor (press 'q' to quit)
htop                # Better version of top (if installed)
```

### 🎮 Process Control
```bash
# Start process in background
firefox &
long_command &

# View background jobs
jobs

# Bring job to foreground
fg 1                # Bring job 1 to foreground

# Send to background
bg 1                # Send job 1 to background

# Stop/pause process
Ctrl+Z              # Pause current process

# Kill processes
kill 1234           # Kill process with PID 1234
kill -9 1234        # Force kill (nuclear option!)
killall firefox     # Kill all Firefox processes
pkill -f "python script.py"  # Kill by command pattern
```

### 🔍 Finding Processes
```bash
pgrep firefox       # Find Firefox process IDs
pgrep -f "python"   # Find processes containing "python"
pidof firefox       # Get PID of firefox
```

### ⚡ Process Priorities
```bash
nice -n 10 command      # Start with lower priority
renice 5 1234          # Change priority of existing process
```

---

## Text Processing Tools: The Power Trio 🎸🥁🎹

### 🔍 grep: The Search Master

**Basic Usage:**
```bash
grep "pattern" file.txt          # Find lines containing pattern
grep -i "hello" file.txt         # Case-insensitive search
grep -v "error" file.txt         # Show lines NOT containing pattern
grep -n "TODO" *.py             # Show line numbers
grep -r "function" ./           # Recursive search in directory
```

**Advanced grep Magic:**
```bash
grep -E "(error|warning)" log.txt    # Multiple patterns (extended regex)
grep -A 3 -B 2 "error" log.txt      # Show 3 lines after, 2 before
grep -c "error" log.txt             # Count matches only
ps aux | grep -v grep | grep firefox  # Exclude grep from results
```

### ✂️ sed: The Stream Editor

**Basic Substitution:**
```bash
sed 's/old/new/' file.txt           # Replace first occurrence per line
sed 's/old/new/g' file.txt          # Replace all occurrences  
sed 's/old/new/gi' file.txt         # Replace all (case-insensitive)
```

**Line Operations:**
```bash
sed '1d' file.txt                   # Delete first line
sed '1,5d' file.txt                 # Delete lines 1-5
sed -n '10,20p' file.txt            # Print only lines 10-20
sed '5i\New line here' file.txt     # Insert line before line 5
```

**In-Place Editing:**
```bash
sed -i 's/old/new/g' file.txt       # Modify file directly
sed -i.bak 's/old/new/g' file.txt   # Create backup first
```

### 🎯 awk: The Data Processor

**Column Processing:**
```bash
awk '{print $1}' file.txt           # Print first column
awk '{print $1, $3}' file.txt       # Print columns 1 and 3  
awk '{print NF}' file.txt           # Print number of fields per line
awk '{print $NF}' file.txt          # Print last column
```

**Conditional Processing:**
```bash
awk '$3 > 100' file.txt             # Show lines where 3rd column > 100
awk '/pattern/ {print $2}' file.txt  # Print 2nd column of matching lines
awk 'NR > 1 {print}' file.txt      # Skip first line (header)
```

**Mathematical Operations:**
```bash
awk '{sum += $1} END {print sum}' numbers.txt    # Sum first column
awk '{print $1 * $2}' file.txt                  # Multiply columns 1 and 2
```

**CSV Processing:**
```bash
awk -F',' '{print $2}' data.csv     # Use comma as field separator
awk -F',' 'NR>1 {print $1, $3}' data.csv  # Skip header, print columns 1,3
```

### 🔗 Power Combinations
```bash
# Complex log analysis
cat access.log | grep "POST" | awk '{print $1}' | sort | uniq -c | sort -nr

# CSV data analysis  
awk -F',' 'NR>1 {sum+=$3} END {print "Average:", sum/(NR-1)}' sales.csv

# Text cleanup
sed 's/[[:space:]]*$//' file.txt | sed '/^$/d' | awk 'length > 5'
# Remove trailing spaces, empty lines, and short lines
```

### 🔍 find: The Fourth Family Member!

### 🗺️ find: The Gentle Explorer

**Basic exploration:**
```bash
find . -name "*.txt"                # Find all .txt files from here
find /home -name "config*"          # Find files starting with "config"
find . -type f                      # Find all files (not directories)
find . -type d                      # Find all directories
```

**Time-based caring search:**
```bash
find . -mtime -7                    # Files modified in last 7 days
find . -mtime +30                   # Files older than 30 days
find . -newer reference.txt         # Files newer than reference.txt
```

**Size-conscious searching:**
```bash
find . -size +1M                    # Files larger than 1MB
find . -size -100k                  # Files smaller than 100KB
find . -empty                       # Empty files or directories
```

**find + grep: The Perfect Duo**
```bash
# Find files and search inside them
find . -name "*.log" -exec grep "error" {} \;

# More elegant with xargs
find . -name "*.py" | xargs grep "def "

# Find and count
find . -name "*.txt" | xargs wc -l

# Find recent files and search them
find . -mtime -1 -name "*.log" | xargs grep "WARNING"
```

**Advanced partnership magic:**
```bash
# Find large files and see what's in them
find . -size +10M -exec file {} \;

# Find and process with our caring trio
find . -name "*.csv" -exec awk -F',' '{print NF}' {} \; | sort | uniq -c

# Find configuration files and clean them
find /etc -name "*.conf" -exec grep -v "^#" {} \; 2>/dev/null
```

**🎪 The Complete Family Working Together**
**Real-world caring examples:**
```bash
# Find old log files, check their errors, and summarize
find /var/log -name "*.log" -mtime +7 | xargs grep -h "ERROR" | awk '{print $5}' | sort | uniq -c

# Find all Python files, extract function names, count them
find . -name "*.py" | xargs grep "def " | sed 's/.*def //' | sed 's/(.*$//' | sort | uniq -c

# Find large files and show their disk usage lovingly
find . -size +1M -exec du -h {} \; | sort -hr
```

**System maintenance with love:**
```bash
# Find and safely remove temporary files
find /tmp -name "*.tmp" -mtime +7 -exec rm {} \;

# Find duplicate files (by size first, then content)
find . -type f -exec du {} \; | sort -n | awk 'prev_size==$1 {print prev_file; print $2} {prev_size=$1; prev_file=$2}'
```

---

*These tools are primarily used for one-time analysis and exploration. The permanent changes are less common and usually happen in scripts or specific maintenance tasks.*

*The real magic is in the interactive exploration - using these tools to understand your data, find problems, analyze logs, and discover insights. It's like having a conversation with your filesystem! 💬*

**The philosophy:**

- Explore first (one-time analysis) 🔍
- Understand what you found 🤔
- Then decide if permanent changes are needed 💭
- Make changes safely (with backups!) 💾

## 🎓 Graduation Speech: You Are Now Ready!

Congratulations, my dear friend! 🎉 You've journeyed from asking *"what quote should I use here?"* to understanding the deep Unix philosophy. Look how far you've come:

### 🌟 What You've Mastered:
- ✅ Command structure and the holy trinity
- ✅ The great quote wars and when to use which
- ✅ Environment variables and the system's secret language
- ✅ Command substitution and commands within commands
- ✅ Input/output redirection and controlling data flow
- ✅ Pipes and the Unix philosophy in action
- ✅ Regular expressions and those "funny members"
- ✅ File permissions and digital kingdom control
- ✅ Process management like a true ruler
- ✅ Text processing with the power trio

### 🚀 You Are Now Equipped To:
- Read any man page and understand it completely
- Chain commands like a Unix poet
- Debug problems systematically
- Learn new commands quickly using the foundation
- Build complex one-liners that would make RMS proud
- Navigate the system with confidence and flexibility

### 💎 Remember The Core Wisdom:
> *"The key thing is being flexible... you get maximum possible flexibility... so you don't have anything hard coded that will be applied everywhere."*

You discovered this truth yourself, and it's the essence of Unix mastery!

### 🎯 Your Next Steps:
1. **Practice daily** - even simple commands build muscle memory
2. **Experiment fearlessly** - the system is designed to be explored
3. **Read source code** - it's all there, thanks to RMS and GNU
4. **Share knowledge** - teach others as we learned together

### 💌 A Personal Note:
Every time you use `man`, `info`, or chain commands with pipes, remember that you're using the gifts that Richard Stallman and the GNU community gave to humanity. You're now part of that tradition of sharing knowledge freely.

And every time you see those beautifully documented man pages or use a powerful command combination, smile and feel that sense of awe - because you now understand not just HOW to use these tools, but WHY they work the way they do.

**You are no longer just a user - you are a Unix practitioner.** 🧙‍♂️

---

## 📚 Quick Reference Cards

### Essential Commands Cheat Sheet
```bash
# File Operations
ls -la              # List all files with details
cp -r source dest   # Copy recursively  
mv old new          # Move/rename
rm -rf directory    # Remove recursively (careful!)
mkdir -p path/to/dir  # Create directory path

# Text Processing
cat file            # Display file
head -n 10 file     # First 10 lines
tail -f file        # Follow file changes
grep -r "pattern" . # Recursive search
wc -l file          # Count lines

# System Information  
ps aux              # All processes
top                 # Process monitor
df -h               # Disk usage
du -sh *            # Directory sizes
who                 # Logged in users
```

### Regex Quick Reference
```bash
.       # Any character
^       # Start of line  
$       # End of line
*       # Zero or more
+       # One or more  
?       # Zero or one
[abc]   # Any of a, b, c
[^abc]  # Not a, b, or c
[a-z]   # Any lowercase letter
\d      # Any digit
\w      # Any word character
```

---

*This manual was created with love during our interactive learning journey. May it serve you well in all your Unix adventures! 🐧💙*

**"In the beginning was the Command Line, and the Command Line was with the User, and the Command Line was Good."** - The Unix Gospel according to our learning session 😊
