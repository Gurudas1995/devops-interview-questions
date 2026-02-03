# Linux Interview Questions and Answers

---
### Table of Contents

<details open>
<summary>
Hide/Show table of contents
</summary>

| No. | Questions |
| --- | --------- |
| 1   | [List All git Commands.](#List-All-git-Commands) |
| 2   | [What are the benefits of DevOps?](#what-are-the-benefits-of-devops) |
| 3   | [What is Continuous Integration?](#what-is-continuous-integration) |



1. ### How do you check open ports in Linux?
Using `netstat -tuln` or `ss -tuln`.

2. ### How do you check the default gateway in linux?
Using `ip route show` or `route -n`.

3. ### I want to delete folders and directories which are older than 10 days from my linux system, which command to run to delete this older files?
   I will use `find` command with `-mtime +10` to identify files or directories older than 10 days, then I will use `-delete` or `-exec rm -rf` to clean up older directories.
 - Command to list the directories:
   ```
   $ find /path/to/directory -type d -mtime +10
   ```
 - Command to delete directories:
   ```
   $ find /path/to/directory -type d -mtime +10 -exec rm -rf {} + #rm -rf is very destructive
   OR
   $ find /path/to/directory -type d -mtime +10 -delete #safer than above
   ```
4. ### explain du-sh and df -h command.
    - `du -sh` shows how much disk space a file or directory is actually using.
      ```
      $ du-sh <file or directory>
      ```
    - `df -h` shows total disk size, used space, and free space of mounted filesystems.
     ```
     $df -h
     ```
    - When a server reports disk full, I first use `df -h` to identify which filesystem is full. Then I use `du -sh`
 to drill down into directories and find which files or folders are consuiming space.
  
   5. ### One of my disk is overutilized in linux server and I want to increase the disk size what command should I use?
   - First, I check disk usage with `df -h` and disk layout using `lsblk`. If it's an LVM disk, I extend the partition using `growpart`, resize the physical volume with `pvresize`, extend the logical volume using `lvextend`, and finally resize the filesystem using `resize2fs` or `xfs_growfs`.
   - This can be done online without downtime.
   - Identify disk and partition type with below command, which tells you disk name (/dev/sda, /dev/nvme0n1), partition (/dev/sda1), LVM or non-LVM and mount point.
     ```
     lsblk #scan the disks
     ```
   - *Case 1: Disk is LVM (Most Common in cloud servers)*
     - After increasing disk from cloud (AWS/Azure/GCP), rescan disk
     - Extend the partition
       ```
       growpart /dev/sda 2
       ```
     - Resize physical volume (PV)
       ```
       pvresize /dev/sda2
       ```
     - extend logical volume (LV)
       ```
       lvextend -L +20G /dev/mapper/vgname-lvname
       ```
     - Resize filesystem
       - For ext4
         ```
         resize2fs /dev/mapper/vgname-lvname
         ```
       - for xfs (most RHEL/CentOS)
         ```
         xfs_growfs /
         ```
       - lastly verify it using `df -h`
    - *Case 2: Non-LVM disk (Simple partition)*
       - grow partiton
         ```
         growpart /dev/sda 1
         ```
      - Resize filesystem
        ```
        resize2fs /dev/sda1
        ```
      - xfs
        ```
        xfs_growfs /mountpoint
        ```
  - *Case 3: New disk attached*
    - Create partition
      ```
      fdisk /dev/sdb
      ```
    - Format
      ```
      mkfs.xfs /dev/sdb1
    - mount
      ```
      mount /dev/sdb1 /data
      ```
    - Persist mount
      ```
       echo "/dev/sdb1 /data xfs defaults 0 0" >> /etc/fstab
      ```
      ---
      6. ### Disk shows 100% full but `du` doesn't show large files
      -   - Sometimes `df -h` shows disk full and `du -sh` does not show large files. Reason is deleted files still opened by running procesess. To fix this we can use below command. 
      ```
      lsof | grep deleted
      ```
      Then restart or kill the process:
      ```
      kill -9 <pid>
      ```
      ---
7. ### Linux Server is slow but CPU usgae looks normal
   Even if CPU is low, high I/O wait or swap usage (memory pressure) or zombie processes can slow the system. I use `vmstat` and `iostat` to confirm.

8. ### Load average is high but CPU usage is low, why?
 Load average includes running processes and processes waiting for disk or I/O. To check we can use `top`. High load with low CPU useually means I/O bottleneck, not CPU contention.

9. ### Application works manually but fails in CRON what will be the reason?
Cron has limited environment variables. Cron doesn't load user profiles, so commands fail unless paths and variables are explicitly defined.

10. ### Root filesystem is full, system is unstable how to fix it?
I first stabilize the system by clearing logs or temp files, then permanently fix via disk extension or log rotation.

11. ### High memory usage but no big processes present what will be the cause?
Linux uses memory for buffers/cache. we can check it using `free -h`.
High memory usage isn't an issue unless swap is used heavily. Linux releases cache automatically when needed.

    
    
      
       
   

    
  



