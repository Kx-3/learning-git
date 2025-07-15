# Lesson: Introduction to Linux and Bash Scripting

## 1. Introduction to Linux

### 1.1 Brief History of Linux
- Linux is an open-source operating system that is based on the Unix operating system. It was created by Linus Torvalds in 1991.
- Gained popularity due to its stability, security, and flexibility.

### 1.2 Features of Linux
- **Open Source**: Free to use, modify, and distribute.
- **Multi-User**: Supports multiple users simultaneously.
- **Security**: Built-in robust security features.
- **Portability**: Runs on various hardware platforms.
- **Customizability**: Highly customizable with various desktop environments.

### 1.3 Linux Distributions and Use Cases
- **Ubuntu**: User-friendly, ideal for beginners and desktops.
- **CentOS/RHEL**: Enterprise-grade, used for servers.
- **Debian**: Stable and versatile, used for servers and desktops.
- **Kali Linux**: Designed for penetration testing and security.
- **Raspberry Pi OS**: Lightweight, used for IoT and embedded systems.

### 1.4 The reasons to learn Linux in IT
- Linux being the de facto OS powering majority of the servers world wide your application is likely to be hosted on linux server. Learning Linux as DevOps Engineer will be of essence.
- In the age of cloud computing, there is high chance that your cloud will rely on Linux.
- Linux serves as the foundation for many operating systems for the Internet of Things (IoT) and mobile applications.
- In DevOps, server administration is one of the activities you will be engaging. Knowing how to work with linux will be of essence.

### 1.4 Installing Linux
#### 1.4.1 Installation on a PC
- Go to the official [Website](https://ubuntu.com/download) and download the ISO file. Make sure to select a stable release that is labeled "LTS". LTS stands for Long Term Support, which means you can get free security and maintenance updates for a long time (usually 5 years).

- Create a bootable pendrive: There are a number of softwares that can create a bootable pendrive. I recommend using Rufus, as it is quite easy to use. [Here](https://rufus.ie/en/)

- Boot from the pendrive: Once your bootable pendrive is ready, insert it and boot from the pendrive. The boot menu depends on your laptop.

Linux can be installed along other operating systems, ussualy referred ad dual booting
#### Installation windows.
![Default Linux installation Window](../../assets/linux-install.png)


#### 1.4.2 Dual-boot(Windows+Linux)
- With dual boot, you can install Linux alongside Windows on your computer, allowing you to choose which operating system to use at startup.

- This requires partitioning your hard drive and installing Linux on a separate partition. With this approach, you can only use one operating system at a time.

#### 1.4.3 Using WSL (Windows Subsystem for Linux)
- Windows Subsystem for Linux provides a compatibility layer that lets you run Linux binary executables natively on Windows.
#### Advantages of WSL
-  Set up is simple and straight forward.
- It is simple compared to VMs, where allocation for resource on host machine is required.
- You don't need ISO or virtual disk image for linux machines which tend to be heavy files.
2. Managing Files From the Command Line
### 2.1 Basic Linux Commands
All files in Linux are stored in a file-system. The root being the topmost part, it appears as an inverted-tree-like.
As shown on the below diagram, To check on command line/Terminal `tree -d -L 1`

![File System](../../assets/linux-file-system.webp)

Location	Purpose
/bin	Essential command binaries
/boot	Static files of the boot loader, needed in order to start the boot process.
/etc	Host-specific system configuration
/home	User home directories
/root	Home directory for the administrative root user
/lib	Essential shared libraries and kernel modules
/mnt	Mount point for mounting a filesystem temporarily
/opt	Add-on application software packages
/usr	Installed software and shared libraries
/var	Variable data that is also persistent between boots
/tmp	Temporary files that are accessible to all users

| Location      | Purpose                                                                 |
|--------------|-------------------------------------------------------------------------|
| `/bin`       | Essential command binaries                                              |
| `/boot`      | Static files of the boot loader, needed in order to start the boot process. |
| `/etc`       | Host-specific system configuration                                      |
| `/home`      | User home directories                                                   |
| `/root`      | Home directory for the administrative root user                         |
| `/lib`       | Essential shared libraries and kernel modules                           |
| `/mnt`       | Mount point for mounting a filesystem temporarily                       |
| `/opt`       | Add-on application software packages                                    |
| `/usr`       | Installed software and shared libraries                                 |
| `/var`       | Variable data that is also persistent between boots                     |
| `/tmp`       | Temporary files that are accessible to all users                        |




### 2.1.0 Basic Linux Commands.

- `ls`: List directory contents.
- `cd`: Change directory.
- `pwd`: Print working directory.
- `mkdir`: Create a directory.
- `rm`: Remove files or directories.
- `cp`: Copy files or directories.
- `mv`: Move or rename files.
- `cat`: View file contents.
- `chmod`: Change file permissions.

### 2.1.1 Advanced Linux Commands
- `grep`: Search text using patterns.
- `find`: Search for files and directories.
- `tar`: Archive files.
- `curl`: Transfer data from or to a server.
- `top`: Monitor system processes.
- `df`: Display disk space usage.
- `ps`: Display running processes.
- `ssh`: Securely connect to a remote machine.

---

## 3. Introduction to Bash
### 3.0 What is Bash?
Linux command Line is provided by a program called the shell, it has evolved to cater various options over the years.
- Bash (Bourne Again Shell) is a command-line interpreter.
- Default shell for most Linux distributions.
- Used for executing commands and writing scripts.  
`echo $SHELL` is used to find out your current shell.

```
# output
$ echo $SHELL
/bin/bash
```

Terminal vs Shell
These terms are used often interchangeably  but are not the same.<br>
**Terminal**: Is the interface you use to interact with the shell.  
**Shell**: The command interpreter the processes and executes your commands
 **Bash Script**: A bash script is a file containing a sequence of commands that are executed by the bash program line by line.  
 It allows you to perform a series of actions, such as navigating to a specific directory, creating a folder, and launching a process using the command line.

###  3.1 Advantages of Shell/Bash scripting
- Automating repetitive tasks: It saves time and reducing risks that can occur with human execution.
- Portability: The scripts runs on various platforms e.g Linux, Unix, Mac OS and Windows through emulators.
- Flexibility: They can easily be modified to suit specific requirements. Can be integrated with other programming languages to create more powerful scripts.

### 3.2 Use cases
- Automation of tasks
- Managing system configurations.
- Processing text and files.
- Scheduling tasks using cron jobs.
- Deploying applications and managing servers.

### 3.3 Basics of Bash
To execute a bash file, it MUST be save with `.sh` extensions and made executable by running `chmod u+x ./file-name.sh`
#### 3.3.1 Variables
```bash
name="John"
echo "Hello, $name"
```

#### 2.3.2 Conditional Statements
```bash
if [ $age -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

#### 2.3.3 Loops
- **For Loop**:
    ```bash
    for i in {1..5}; do
        echo "Number $i"
    done
    ```
- **While Loop**:
    ```bash
    count=1
    while [ $count -le 5 ]; do
        echo "Count $count"
        ((count++))
    done
    ```

#### 2.3.4 Functions
```bash
greet() {
    echo "Hello, $1!"
}
greet "Alice"
```

#### 2.3.5 File Operations
- Reading a file:
    ```bash
    while read line; do
        echo $line
    done < file.txt
    ```
- Writing to a file:
    ```bash
    echo "Hello, World!" > output.txt
    ```

---

### Summary
- Linux provides a robust, secure, and flexible environment for various use cases.
- Bash scripting enhances productivity by automating tasks and managing systems efficiently.

### Suggested Practice
- Explore basic Linux commands in a terminal.
- Install Linux on a virtual machine or WSL.
- Write a simple Bash script to automate a task (e.g., creating a backup of a directory).
- Experiment with loops and conditional statements in Bash.
- Create a script to process text files or schedule a task using cron.