# Git & GitHub Interview Questions and Answers

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



1. ### How have you used shell scripting in your role?
  - Automation: I have used shell scripting for automaton, resoruce healthchecks and system management.
  - CI/CD pipeline: I have wrote script to ensure necessary directories exist before deployment or to check application health after deployment.
  - Backup automation: Created scripts using 'tar' and 'rsync' to automate nightly backups of specific configuration and logs files.
  - Log Management: Used 'grep', 'awk' and 'sed' within scripts for parsing and analyzing large log files to extract relevant info or errors.

2. ### What is CRON ? How would you schedule job to run everyday at midnight?
   CRON is time-based job schedular in Linux that automates repetative tasks to schedule a job like backups to run daily at midnight, I would use `crontab -e` command to edit the user's crontab file and add following:
   ```
   0 0 * * * /path/to/backup.ssh
   ```
   five astricks represents minute, hour, day of month, month and day of week.

3. ### 
   
