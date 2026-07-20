# **Chapter 3: Process Management**
# **Foreword**

We recommend that you do not use AI to do the exercises since you are in a learning phase.

# **Introduction**

In a Linux system, process management is a fundamental aspect of system administration and application operation. A process is simply a program that is currently running. Linux, like all modern operating systems, uses processes to execute tasks and organize work.

## Prerequisites 

* Have a virtual machine, a PC, or an environment running Linux (ideally Ubuntu)
* Be resilient 😜

**Info:** If you don't have a Linux environment available, you can register at <https://killercoda.com> and go here <https://killercoda.com/playgrounds/scenario/ubuntu> to get access to a virtual machine running Ubuntu 24.04 (without a graphical interface, of course!!) for 1 hour, freely renewable.

You will therefore see this view:  

![](https://raw.githubusercontent.com/N0vachr0n0/Hands-on-Linux-course/refs/heads/main/fr/pictures/killerkoda_vm.png)


<br>
<br>

---

# **General information**

A process is an instance of a program in execution. It can be as simple as a command launched in the terminal or as complex as a background application. Each process has its own memory space, its own unique identifier (PID), and information about its execution state. If process 2 was launched by process 1, it is called a **child process**. The process that launched it is called the **parent process**.

<br>
There are different types of processes in Linux:

* **Interactive processes**: These are processes launched by a user directly via a terminal or graphical interface. For example, launching a text editor or a web browser.

* **Background processes (daemons)**: These are processes that run without direct interaction with the user, often used for system services, such as a web server (Apache, for example).

* **Zombie processes**: These are processes that have finished executing but whose information remains in the process table because their parent has not yet retrieved their exit status.


## TLDR (Quick summary)

* A process is an instance of a program in execution.
* Every process has:
    * a PID: Process IDentifier, a unique process identifier;
    * a PPID: Parent Process IDentifier, a unique identifier for the parent process.
* Types of processes: interactive, background, and zombie


## Let's type a little

**To test 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run: 
  ```bash
  ps
  ```

<br>

For your information, **ps** stands for **p**rocess **s**tatus. It lets us display the currently running processes.
You'll get output similar to: 

```
PID    TTY          TIME CMD
380123 pts/3    00:00:00 bash
427931 pts/3    00:00:00 ps
```

Below is an explanatory table.

| Column | Meaning | Example |
|---------|---------------|---------|
| **PID** | Unique identifier assigned by Linux to each process. | `bash`: **380123**<br>`ps`: **427931** |
| **TTY** | Terminal the process is attached to. `pts/*` are pseudo-terminals, often opened during an SSH connection or a graphical terminal. | `pts/3` |
| **TIME** | CPU time used by the process since it was launched. | `00:00:00` (minimal time for fast or idle processes) |
| **CMD** | Command or program that the process originated from. | `bash` (interactive shell)<br>`ps` (process display command) |


<br>

From this result, we might ask ourselves two (2) questions:
* Why does **ps** display itself? 
* Why don't we see all the system's processes after typing **ps**?

1. The ps command is itself a Linux process. When it is executed, it temporarily creates itself as a process, then analyzes the processes running at that precise moment. That's why it naturally appears in its own results.

2. By default, this command (**ps**) without any particular argument only displays the processes associated with its current terminal. This explains why we only see the shell (bash) we're currently using, as well as the ps command itself.

<br>

**To test 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run: 
  ```bash
  ps
  echo 'foo' > myfile.txt
  tail -f myfile.txt & #To run the task in the background
  jobs #To view background tasks
  ps
  ```

<br>
<br>

---

# **Process management**

## Commands to display running processes 

- **In real time:** `top`  
- **At a given moment:** `ps`  
  - `-e`: all processes  
  - `-f`: detailed display  

**Practical examples:**  

- List all processes with details: `ps -ef`  
- List processes in tree form: `ps -faux`  
- Check whether the apache process is running: `ps -ef | grep httpd`  (apache2=Debian, httpd=RedHat) 
- Stop process `123` cleanly (SIGTERM signal): `kill 123`  
- Force immediate termination of process `123` (SIGKILL signal): `kill -9 123`

<br>

**Reminder:** The **man** command and the **--help** option (two dashes please 🫠) are your best friend.

<br>

## Other commands

### The pgrep and pkill commands

The **pgrep** command searches among the currently running processes for a process name and displays on standard output the PIDs matching the selection criteria.

The **pkill** command sends the specified signal (default **SIGTERM**) to every process matching the specified criteria.

**Syntax:**

- `pgrep process`  
- `pkill [-signal] process`

**Examples:**

- Retrieve the process number of the **sshd** service for the root user:  
  ```bash
  pgrep -u root sshd
  ```

- Kill all tomcat processes:
    ```bash
    pkill tomcat
    ```
    
<br>
<br>

---

# **Training ⚔️**

## Exercise 1

* Link to the challenge script: <https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/PRM_EXO_1.sh>

Below is an example run:

```bash
# Download the challenge 1 script
curl -LO https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/PRM_EXO_1.sh

# Make it executable
chmod +x PRM_EXO_1.sh

# Run it to start the challenge
./PRM_EXO_1.sh
```

## Exercise 2

It's over here: <https://sadservers.com/scenario/saint-john>

---
---

## Feedback


> Let us know what you think of this chapter.

> 👉🏾 [Click here](https://forms.gle/dNjUFEXf6saN8RTEA)

---
---

## Navigation 

* [Previous chapter](./02-basic-commands.md)

* [Next chapter](./04-users-groups.md)

* [Go to the summary](./index.md)
