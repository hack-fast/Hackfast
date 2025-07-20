### **INTRODUCTION**

Cron jobs are programs or scripts that users can schedule to run at specific times or intervals. These tasks are managed by the cron daemon, a background process that executes scheduled commands on Unix-like operating systems. Cron jobs run with the security level of the user who owns them, ensuring that they have the same permissions as the user. This means that a cron job created by a regular user will execute with that user's permissions, while a cron job created by the root user will have root-level privileges.

??? info "NOTE"

    While CTF machines can have cron jobs running every minute or every 5 minutes, you will more often see tasks that run daily, weekly or monthly in penetration test engagements.

### **THERE ARE TWO TYPES OF CRONTABS THAT CAN BE USED TO RUN CRON JOBS**

1.  The system crontab: Used it to schedule system-wide jobs, The default system crontab configuration file is located at /etc/crontab
2.  The user crontab This file lets users create and edit cron jobs that only apply at the user level – Created using the crontab -e command and stored in /var/spool/cron/crontabs
3. crontabs created with crontab -e commands are only visible to the users who create them. They cannot be enumerated in a standard way, which is why they are considered “hidden cron jobs”. Standard users cannot even access the directory where they are stored. The the only way a standard user can view their crontab is by using the crontab command.

### **CRON DIRECTORIES**

cron jobs can be added to the etc/cron.d directory. However, this is bad practice because cron.d is mainly intended for automatic installations and updates, root user can also move their scripts into the following directories to schedule their execution (also bad practice):

1.  Run all scripts once an hour  
    `/etc/cron.hourly/`
2.  Run once a day.  
    `/etc/cron.daily/`
3.  Run once a week.  
    `/etc/cron.weekly/`
4.  Run once a month.  
    `/etc/cron.monthly/`