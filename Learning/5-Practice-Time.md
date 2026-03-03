# ls - List Files

# Basic listing
ls                    # Show files in current directory
ls -l                 # Long format (permissions, size, date)
ls -la                # Long format + hidden files (starting with .)
ls -lh                # Long format + human readable sizes (KB, MB)
ls /home/bijoy        # List files in specific directory


# find - Find Files

# Find by name
find . -name "*.txt"           # Find all .txt files in current directory
find /home -name "document*"   # Find files starting with "document"
find . -type f -name "*.py"    # Find only files (not directories) ending in .py
find . -type d -name "Data*"   # Find only directories starting with "Data"

# Find by size/time
find . -size +100M             # Files larger than 100MB
find . -mtime -7               # Files modified in last 7 days


# grep - Search Inside Files

# Basic search
grep "hello" file.txt          # Find "hello" in file.txt
grep -i "HELLO" file.txt       # Case insensitive search
grep -r "function" .           # Search "function" in all files recursively
grep -n "error" log.txt        # Show line numbers
grep -v "debug" log.txt        # Show lines that DON'T contain "debug"

# Combine with other commands
ls -la | grep "Jul"            # Show only July files
history | grep "apt"           # Find apt commands in history


# Practice time! Try these on your beloved Ubuntu:

# Find all Python files in your home
find ~ -name "*.py" | head -10

# Search for "anaconda" in your bash history
grep -i anaconda ~/.bashrc

# List only directories in current folder
ls -la | grep "^d"

