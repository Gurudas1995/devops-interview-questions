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
Using `netstat -tuln` or `ss -tuln`. The 'lsof -i' command also works well. (-t for TCP, -u for UDP, -l for listning port, -n for Numeric address).

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
 Load average includes running processes and processes waiting for disk or I/O. To check we can use `top` or 'htop'. High load with low CPU useually means I/O bottleneck, not CPU contention.

9. ### Application works manually but fails in CRON what will be the reason?
Cron has limited environment variables. Cron doesn't load user profiles, so commands fail unless paths and variables are explicitly defined.

10. ### Root filesystem is full, system is unstable how to fix it?
I first stabilize the system by clearing logs or temp files, then permanently fix via disk extension or log rotation.

11. ### High memory usage but no big processes present what will be the cause?
Linux uses memory for buffers/cache. we can check it using `free -h`.
High memory usage isn't an issue unless swap is used heavily. Linux releases cache automatically when needed.

12. ### What is Linux boot process and why is it important for DevOps?
    The Linux boot process is the series of steps the system takes from the moment you hit the power button until the OS is fully functional and ready for users. below are the 6 stages for Boot process.
 - **BIOS/UEFI**: The hardware performs a POST(Power-On-Self-Test) and looks for a bootable device like a hard drive or SSD.
 - **MBR/GPT**: The system executes the Boot Loader code located in the first sector of the disk.
 - **GRUB(Grand Unified Bootloader)**: This is the menu where you select your OS. It loads the Kernel and the initrd/initramfs (a temp file system) into memory.
 - **Kernel**: The "brain" of the OS initializes hardware drivers and starts the very first process, known as init or systemd.
 - **init(systemd)**: The parent of all processes. It looks at the target like graphical or multi-user mode and starts the necessary background services (daemons).
 - **Runlevel/Target**: The system reaches its final state (e.g. a login screen or a command-line prompt).

It matters for DevOps for below points:
 - **IaC**: When we automate server spin-ups using tools like terraform, understanding the boot process helps you troubleshoot why an instance fails to initialize.
 - **Performance Tuning**: DevOps engineers often need to optimize boot times for auto-scaling groups knowing which services start when allows you to strip out unncessary stpes.
 - **Troubleshooting and Security**: If a production server goes down and won't restart, you need to know if the issue is a corrupted FRUB config, a missing Kernel module, or a failed systemd service.

13. ### Explain purpose of 'Inode' in Linux.
    An 'Inode (Index Node)' is a fundamental data structure used by filesystem to store metadata about a file or directory and pointers to its actual data blocks. Every file and directory on a Linux system has one unique inode. Inode serve below critical purposes:
    - **Metadata Storage**: They contain file details like permisson, owner, size, timestamps, and link count excluding the filename and data content.
    - **Data Location**: They hold pointers to the physical disk blocks containing the actual file data.
    - **Decoupling**: By separating the filename in directories from the metadata, Linux allows featureslike hard links and easy file renaming.

14. ### How do you find and all kill a process that is consuiming too much memory or CPU?
    Use 'top' or 'htop' to idnetify process ID (PID) of resource-hungry process. then use 'kill' command with PID. By default, 'kill <PID>' sends a graceful termination signal. If the process doesn't stop, use 'kill -9 <PID>' to forcefully terminate it.

15. ### How do you securely transfer files between two linux servers?
    Using 'SCP' or 'rsync' over SSH.
    - SCP is ideal for simple direct file copies e.g. SCP file.txt user@remote:/path/
    - rsync is better for syncing directories, handling incremental trasfers and preserving file attributes.
    - Both leverages SSH encryption for security.

16. ###
    
    

    
    
      
       
   

    
  



