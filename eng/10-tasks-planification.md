# Foreword
We recommend that you do not use AI to do the exercises, as you are still in the learning phase.
# Introduction
In this chapter, we will explore **task scheduling** in Linux, an essential concept for automating repetitive processes and optimizing system management. Whether for backups, cleaning temporary files, or periodic checks, task scheduling is a key skill for any system administrator or Linux user. We will focus on **cron**, the most widely used and powerful scheduling tool in Linux. Get ready to become a master of automation, dear padawan!

---

## Prerequisites

Same old story. 😉

---

<br>
<br>

# What is task scheduling?
**Task scheduling** allows you to automatically execute commands or scripts at specific times or at regular intervals. Imagine an alarm clock that, instead of ringing, executes a task on your system: backing up files, sending reports, or cleaning up folders. This frees you from repetitive tasks and ensures that your system stays up to date without manual intervention.
In Linux, there are several tools available for scheduling tasks:
- **cron**: The standard, flexible, and robust tool, perfect for periodic tasks.
- **at**: For one-time tasks at a specific time.
- **anacron**: Ideal for tasks that must run even if the system was shut down at the scheduled time.
In this course, we will focus on **cron**, the preferred choice for recurring automation.
---
## Cron
### What is cron?
**cron** is a **daemon** (a program that runs in the background) that reads configuration files called **crontabs** to execute scheduled tasks. Each user can have their own **crontab**, and there is also a **system crontab** for global tasks. Think of **cron** as a conductor who ensures that each task is performed at the right time.
### Crontab structure
The **crontab** file consists of lines, each representing a scheduled task. Each line follows this structure with **six fields**:
```
* * * * * command 2>&1 >/dev/null
^ ^ ^ ^ ^
| | | | |
| | | | +­­ day of the week 0-6
| | | +­­­­ month 1-12
| | +­­­­­­ day of the month 1-31
| +­­­­­­­­ hour 0 23
+­­­­­­­­­­ minute 0-59
```
**Note**: **/dev/null** is a file that points to nothing, so the command output is sent nowhere. This allows you to get rid of the output so that it is not logged—especially since, without redirection, the standard output is sent by email to root. That said, you can redirect the output to a log file to keep yourself informed of any errors that may occur during a scheduled task, or use the chronic command.
Each field can contain:
- A specific value (e.g., `15` for 15 minutes).
- An asterisk (`*`) for “all possible values.”
- A list (e.g., `1,3,5`).
  
- A range (e.g., `1-5`).
- An interval (e.g., `*/5` for every 5 minutes).
### Examples of cron tasks
Here are some concrete examples to help you understand:
- **Every day at 2:30 p.m.**:
```bash
30 14 * * * /usr/bin/backup.sh
  
```
  Runs the backup.sh script at 2:30 p.m. every day.
- **Every Monday at 10:00 a.m.**:  
```bash
    0 10 * * 1 /usr/bin/clean_logs.sh
    ```
Runs clean_logs.sh every Monday at 10:00 a.m.
    
- On the 1st of every month at 12:00 p.m.:  
```bash
    0 12 1 * * /usr/bin/monthly_report.sh
```
Runs monthly_report.sh on the 1st day of every month at noon.
- Every hour:  
```bash
    0 * * * * /usr/bin/check_status.sh
    
```
    Runs check_status.sh at the start of every hour (12:00 a.m., 1:00 a.m., etc.).
- Every day at midnight:  
```bash
    0 0 * * * /usr/bin/daily_backup.sh
    ```
Runs daily_backup.sh every day at 12:00 a.m.


**Learning tip**: Use the online tool crontab guru (https://crontab.guru/) to test and view cron expressions in real time.
### Editing the crontab file
To manage a user's cron jobs, use the crontab command:
`crontab -e`: Opens the user's crontab file in the default editor (e.g., nano or vi).  
`crontab -l`: Displays the scheduled tasks in the user's crontab.
  
`crontab -r`: Deletes all of the user's scheduled tasks (caution: this cannot be undone!).





**To test 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run:
```bash
$ crontab -e ## Opens the crontab
```
- Add a line: */10 * * * * /home/user/script.sh.
- Save and exit. The cron daemon automatically detects changes.


(( I hope you created the script file 😝))

**Note**: The system crontab (for root or global tasks) is located in /etc/crontab or /etc/cron.d/. It includes an additional field to specify the user executing the command.

### Time specifiers
To simplify certain schedules, cron offers special specifiers:
    @reboot: Executes the task once when the system starts up.
      
@yearly: Runs once a year (equivalent to 0 0 1 1 *).  
    @monthly: Runs once a month (equivalent to 0 0 1 * *).
  
    @weekly: Runs once a week (equivalent to 0 0 * * 0).
      
@daily: Runs once per day (equivalent to 0 0 * * *).  
    @hourly: Runs once per hour (equivalent to 0 * * * *).
**Example:**
To initialize a service at startup:
```bash
@reboot /usr/bin/start_service.sh
```
## Tips and best practices
To avoid common mistakes and optimize your cron jobs:
- **Use absolute paths**: Environment variables in cron are limited. Always specify full paths (e.g., /usr/bin/echo instead of echo).
- **Redirect output**: Cron jobs send their output (stdout/stderr) to the user via email. Redirect them to a file to avoid this:  
    ```bash
    0 0 * * * /usr/bin/script.sh >> /var/log/script.log 2>&1
    ```
- **Test your scripts beforehand**: Run your script manually to make sure it works correctly before adding it to crontab.
- **Check permissions**: Make sure the scripts are executable (chmod +x script.sh) and that the cron user has the necessary permissions.
- **Use logs**: Record the results of your tasks in log files to make debugging easier.
- **Monitor cron**: Check the system logs (/var/log/cron or /var/log/syslog) to see if your tasks are running correctly.
- **Avoid overlaps**: If a task can take a long time, use a lockfile to prevent multiple simultaneous executions.



**Example script with logging**:  
```bash
#!/bin/bash
echo “Task started at $(date)” >> /var/log/task.log
# Task here
echo “Task completed at $(date)” >> /var/log/task.log
```

### Quick explanation!
**“Avoid overlaps”**: What exactly does this mean?
When you schedule a task at regular intervals  (for example, via cron), the same task may be restarted before the previous one has finished .
This can cause problems: duplication of processing, system overload, unexpected errors, etc.
Let's imagine you have a cron task that runs every 5 minutes, but sometimes it takes more than 5 minutes to finish.
**Result:** two instances of the same script are running at the same time. This can create a conflict!
**Question:** How can this be avoided?
**Answer:** Use a lockfile.
**Principle of a script using a lockfile:**
1. At the beginning of the script:
It checks whether a lock file (e.g., “/var/lock/myscript.lock”) exists.
     
2. If the file exists:
The script stops → another instance is already running.
 
3. If the file does not exist:
It creates the lock file.
It executes the task.
 
4. At the end:
It deletes the lock file.

**Simple example of a Bash script:**
```bash
#!/bin/bash
LOCKFILE=“/var/lock/monscript.lock”
# Check if the lock file exists
if [ -f “$LOCKFILE” ]; then
  echo “Another instance is already running. Stopping.”
  exit 1
fi
# Create the lock file
touch $LOCKFILE
# Main task code
echo “Task in progress...”
sleep 10
# Delete the lock file at the end
rm -f $LOCKFILE
```

<br>
<br>

# Practice ⚔️

To put your cron knowledge into practice, here are three progressive exercises to master task scheduling.
 
## Exercise 1: Simple cron task
Create a cron task that runs a script every 5 minutes to record the date and time.

## Exercise 2: Daily task with error handling
Schedule a daily task to clean up temporary files and handle errors in a log file.
    
## Exercise 3: Advanced task with dependencies
Set up two dependent cron tasks for backup and verification.

---
---

**And that's how the adventure ends!

Thank you for taking this course. I hope it has helped you better understand the world of Linux. 🙂​**

---
---
## Feedback
> ENG: Please give us your feedback about this chapter.
> FR: Let us know what you think about this chapter.
> 👉🏾 https://forms.gle/Br22WxcwgJSeLGkW9