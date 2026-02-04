## This file is for frquently asked and popular scripts

1. ### File Backup Script
   ```
   #! /bin/bash

   backup_dir="/path/to/backup"
   source_dr="/path/to/source

   tar -czf "$backup_dir/backup_$(date + %Y%m%d_%H%M%S).tar.gz"   # Create a timestamped backup of the source directory
   "$source_dir"
   ```
2. ### System Monitoring Script:
   ```
   #! /bin/bash

   threshold=90
   cpu_usgae=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1) # Monitor CPU usage and trigger alert if threshold exceeded 
   if [ "$cpu_usage" -gt "$threshold" ]; then
     echo "High CPU usage deteted: $cpu_usage%"
   fi
   ```
3. ### User Account Management script
   ```
   #! /bin/bash

   username="newuser"

   if id "$username" &>/dev/null; then
     echo "$username already exists."
   else
     useradd -m "$username"
     echo "User $username created."
   ```
   
   
   

