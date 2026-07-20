# **Chapter 10: Task Scheduling**
# **Foreword**

We recommend that you do not use AI to do the exercises, as you are still in the learning phase.

# **Introduction**

In this chapter, we will explore **task scheduling** under Linux, an essential concept for automating repetitive processes and optimizing the management of a system. Whether for backups, cleaning up temporary files, or periodic checks, task scheduling is a key skill for any system administrator or Linux user. We will focus on **cron**, the most widespread and powerful scheduling tool under Linux. Get ready to become a master of automation, dear padawan!

---

## Prerequisites 

Same story as always. 😉

<br>
<br>

---

# **What is task scheduling?**

**Task scheduling** allows you to automatically execute commands or scripts at precise moments or at regular intervals. Imagine an alarm clock that, instead of ringing, executes a task on your system: backing up files, sending reports, or cleaning up folders. This frees you from repetitive tasks and guarantees that your system stays up to date without manual intervention.

Under Linux, several tools exist for scheduling tasks:
- **cron**: The standard tool, flexible and robust, perfect for periodic tasks.  
- **at**: For one-off tasks at a precise time.  
- **anacron**: Ideal for tasks that must run even if the system was off at the scheduled time.

In this course, we will focus on **cron**, the preferred choice for recurring automation.

---

## Cron

### What is cron?

**cron** is a **daemon** (a program that runs in the background) that reads configuration files called **crontabs** to execute scheduled tasks. Each user can have their own **crontab**, and there is also a **system crontab** for global tasks. Think of **cron** as a conductor who makes sure each task is played at the right moment.

### Crontab structure

The **crontab** file is made up of lines, each representing a scheduled task. Each line follows this structure with **six fields**:

```
* * * * * command  >/dev/null 2>&1
^ ^ ^ ^ ^
| | | | |
| | | | +­­ day of the week 0­6
| | | +­­­­ month 1­12
| | +­­­­­­ day of the month 1­31
| +­­­­­­­­ hour 0 23
+­­­­­­­­­­ minute 0­59
```

**Note**: **/dev/null** is a file that points to nothing, so the command output is sent nowhere. This lets you get rid of the output so it isn't logged - especially since, without redirection, standard output is sent by mail to root. That said, you can redirect the output to a log file to keep yourself informed of errors that may occur during a scheduled task, or use the chronic command.

Each field can contain:
- A specific value (e.g., `15` for 15 minutes).  
- An asterisk (`*`) for "all possible values".  
- A list (e.g., `1,3,5`).  
- A range (e.g., `1-5`).  
- An interval (e.g., `*/5` for every 5 minutes).

### Examples of cron tasks

Here are some concrete examples to help you understand better:

- **Every day at 2:30 pm**:  
  ```bash
  30 14 * * * /usr/bin/backup.sh
  ```

  Runs the backup.sh script at 2:30 pm every day.

- **Every Monday at 10:00 am**:  
    ```bash
    0 10 * * 1 /usr/bin/clean_logs.sh
    ```

    Runs clean_logs.sh every Monday at 10:00 am.
    
- **On the 1st of every month at 12:00 pm:**  
   ```bash
    0 12 1 * * /usr/bin/monthly_report.sh
   ```

    Runs monthly_report.sh on the 1st day of every month at noon.

- **Every hour:**  
    ```bash
    0 * * * * /usr/bin/check_status.sh
    ```

    Runs check_status.sh at the start of every hour (00:00, 01:00, etc.).

- **Every day at midnight:**  
    ```bash
    0 0 * * * /usr/bin/daily_backup.sh
    ```

    Runs daily_backup.sh every day at 00:00.

<br>

**Teaching tip**: Use the online tool crontab guru (https://crontab.guru/) to test and visualize cron expressions in real time.

### Editing the crontab file

To manage a user's cron tasks, use the crontab command:

`crontab -e`: Opens the user's crontab file in the default editor (e.g., nano or vi).  

`crontab -l`: Displays the scheduled tasks in the user's crontab.  

`crontab -r`: Deletes all of the user's scheduled tasks (careful, this is irreversible!).

<br>
<br>


**To test 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run: 
    ```bash
    $ crontab -e ## Opens the crontab 
    ```
- Add a line: */10 * * * * /home/user/script.sh.
- Save and exit. The cron daemon automatically detects changes.
<br>

(( I hope you created the script file 😝))


**Note**: The system crontab (for root or global tasks) is found in /etc/crontab or /etc/cron.d/. It includes an extra field to specify the user executing the command.


### Time specifiers

To simplify certain schedules, cron offers special specifiers:

    @reboot : Executes the task once when the system starts up.  
    @yearly : Executes once a year (equivalent to 0 0 1 1 *).  
    @monthly : Executes once a month (equivalent to 0 0 1 * *).  
    @weekly : Executes once a week (equivalent to 0 0 * * 0).  
    @daily : Executes once a day (equivalent to 0 0 * * *).  
    @hourly : Executes once an hour (equivalent to 0 * * * *).

**Example:**

To start up a service at boot:  

```bash
@reboot /usr/bin/start_service.sh
```

## Tips and best practices

To avoid common mistakes and optimize your cron tasks:

- **Use absolute paths**: Environment variables in cron are limited. Always specify full paths (e.g., /usr/bin/echo instead of echo).  

- **Redirect the outputs**: Cron tasks send their outputs (stdout/stderr) by email to the user. Redirect them to a file to avoid this:  
    
    ```bash
    0 0 * * * /usr/bin/script.sh >> /var/log/script.log 2>&1
    ```
    

- **Test your scripts beforehand**: Run your script manually to make sure it works correctly before adding it to the crontab.

- **Check the permissions**: Make sure the scripts are executable (chmod +x script.sh) and that the cron user has the necessary permissions.

- **Use logs**: Record the results of your tasks in log files to make debugging easier.

- **Monitor cron**: Check the system logs (/var/log/cron or /var/log/syslog) to see if your tasks are running correctly.

- **Avoid overlaps**: If a task can take a while, use a lock file (lockfile) to prevent several simultaneous executions.


<br>

**Example script with logging**:  
```bash

#!/bin/bash
echo "Task started at $(date)" >> /var/log/task.log
# Task here
echo "Task completed at $(date)" >> /var/log/task.log
```


### Quick mini-explanation!

**"Avoid overlaps"**: What exactly is that?

When you schedule a task at regular intervals (for example via cron), it can happen that the same task gets relaunched before the previous one has finished.
This can cause problems: duplicated processing, system overload, unexpected errors...

Let's imagine you have a cron task that runs every 5 minutes, but sometimes it takes more than 5 minutes to finish.
**Result:** two instances of the same script run at the same time. This can therefore create a conflict! 

**Question:** How do you avoid this?

**Answer:** Use a lock file (lockfile).

**Principle of a script using a lockfile:**

1. At the start of the script:

    It checks whether a lock file ("/var/lock/myscript.lock", for example) exists.
     

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

LOCKFILE="/var/lock/myscript.lock"

# Check whether the lock file exists
if [ -f "$LOCKFILE" ]; then
  echo "Another instance is already running. Stopping."
  exit 1
fi

# Create the lock file
touch $LOCKFILE

# Main task code
echo "Task in progress..."
sleep 10

# Delete the lock file at the end
rm -f $LOCKFILE

```

<br>

---

# **Training ⚔️**

To put your knowledge of cron into practice, here are three progressive exercises to master task scheduling. 

## Exercise 1: Simple cron task

Create a cron task that runs a script every 5 minutes to record the date and time.


## Exercise 2: Daily task with error handling

Schedule a daily task to clean up temporary files and handle errors in a log file.

    
## Exercise 3: Advanced task with dependencies

Set up two dependent cron tasks for a backup and a check.

<br>
<br>

---
---

**And that's how the adventure ends!<br>
Thank you for taking this course. I hope it helped you better understand the world of Linux.🙂​**

---
---

## Feedback

> Let us know what you think about this chapter.

> 👉🏾 [Click here](https://forms.gle/Br22WxcwgJSeLGkW9)


---
---

## Navigation 

* [Previous chapter](./09-firewall.md)

* [Next chapter](./11-extra.md)

* [Go To Summary](./index.md)
