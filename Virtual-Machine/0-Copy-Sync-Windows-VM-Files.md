```bash
# (Syncing files from Windows --> Fedora) -- Keep this flow only Windows --> Fedora

rsync -av --no-perms --no-owner --no-group --chmod=D755,F644 --dry-run --delete /mnt/windows-vm/Users/bijoy/OneDrive/Documents/Windows-Analytics/ /home/bijoy/Documents/Windows-Analytics/



# Remove Execute from Files, Keep for Directories (If any found):

find ~/Documents/Windows-Analytics -type f -exec chmod -x {} \;

# OR

find ~/Documents/Windows-Analytics -type f -exec chmod 644 {} \;
```
