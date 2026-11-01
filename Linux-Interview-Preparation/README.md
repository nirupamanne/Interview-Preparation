# Linux Interview Preparation

This document contains Linux interview questions and answers for a DevOps Engineer with around 5 years of overall IT experience.

The answers are written in simple, human-readable language so they are easy to understand and explain during interviews.

## 1. Linux Basics

### 1. What is Linux?

Linux is an open-source operating system. It is widely used in servers, cloud environments, and DevOps because it is stable, secure, and reliable.

**Interview answer:**

"I have worked with Linux servers for application deployment, troubleshooting, log checking, user management, and monitoring. I regularly use Linux commands to manage servers and investigate issues."

### 2. What is the difference between Linux and Windows?

- Linux is open-source, while Windows is a commercial operating system.
- Linux is commonly managed through the command line.
- Windows commonly provides a graphical interface.
- Linux is widely used for cloud servers and DevOps environments.

### 3. What is a Linux kernel?

The kernel is the core part of the Linux operating system. It manages hardware, memory, CPU, processes, and communication between software and hardware.

### 4. What is a Linux shell?

A shell is an interface between the user and the operating system. We use it to execute commands and scripts.

Examples: Bash, Zsh and Sh.

### 5. What is root user?

Root is the administrator account in Linux. It has permission to perform almost all administrative operations.

### 6. What is the difference between root and a normal user?

- Root can perform administrative operations.
- A normal user has limited permissions.
- We can use `sudo` to execute permitted administrative commands.

### 7. What is the Linux file system?

Linux stores files and directories in a hierarchical structure starting from the root directory `/`.

Some important directories:

- `/etc` – Configuration files.
- `/var` – Logs and variable data.
- `/home` – User home directories.
- `/root` – Root user's home directory.
- `/tmp` – Temporary files.
- `/boot` – Boot-related files.
- `/usr` – User applications and utilities.

### 8. What is the difference between an absolute path and a relative path?

- Absolute path starts from `/`.
- Relative path starts from the current working directory.

Example:

```bash
/home/alice/notes.txt
```

This is an absolute path.

```bash
documents/notes.txt
```

This is a relative path.

### 9. How do you check the Linux distribution and version?

I use:

```bash
cat /etc/os-release
```

### 10. How do you check the current logged-in user?

```bash
whoami
```

### 11. How do you check the current directory?

```bash
pwd
```

### 12. How do you check the system hostname?

```bash
hostname
```

## 2. File and Directory Management

### 13. How do you create a directory?

```bash
mkdir devops
```

### 14. How do you create a file?

```bash
touch notes.txt
```

### 15. How do you copy a file?

```bash
cp source.txt backup.txt
```

### 16. How do you move or rename a file?

```bash
mv old.txt new.txt
```

The `mv` command is used to move or rename files and directories.

### 17. How do you delete a file and directory?

```bash
rm file.txt
rmdir empty-directory
```

For a directory containing files:

```bash
rm -r directory
```

### 18. How do you find a file in Linux?

```bash
find /home -name "notes.txt"
```

### 19. How do you check hidden files?

```bash
ls -la
```

Files starting with a dot (`.`) are hidden by default.

## 3. File Permissions and Ownership

### 20. What are Linux file permissions?

Linux has three basic permission types:

- `r` – Read
- `w` – Write
- `x` – Execute

Permissions are assigned to three categories:

- User/owner
- Group
- Others

### 21. What is chmod?

`chmod` is used to change file or directory permissions.

Example:

```bash
chmod 755 script.sh
```

Meaning:

- Owner: read, write, execute.
- Group: read, execute.
- Others: read, execute.

### 22. What is chown?

`chown` changes the owner of a file or directory.

```bash
sudo chown alice file.txt
```

### 23. What is the difference between chmod and chown?

- `chmod` changes permissions.
- `chown` changes ownership.

### 24. What does permission 777 mean?

It means read, write, and execute permissions for the owner, group, and others.

We should avoid using `777` unnecessarily because it gives everyone full access.

### 25. What is umask?

`umask` controls the default permissions assigned when new files and directories are created.

## 4. User and Group Management

### 26. How do you create a user?

```bash
sudo useradd alice
```

### 27. How do you set a password for a user?

```bash
sudo passwd alice
```

### 28. How do you check user information?

```bash
id alice
```

### 29. How do you create a group?

```bash
sudo groupadd devops-team
```

### 30. How do you add a user to a group?

```bash
sudo usermod -aG devops-team alice
```

The `-aG` option adds the user to a supplementary group without removing existing supplementary group memberships.

### 31. Where are Linux user details stored?

- `/etc/passwd` – User account information.
- `/etc/shadow` – Password hashes and password-aging information.
- `/etc/group` – Group information.

### 32. How do you give a user sudo access?

On distributions that use the `wheel` group:

```bash
sudo usermod -aG wheel alice
```

On Ubuntu and Debian:

```bash
sudo usermod -aG sudo alice
```

The correct group depends on the Linux distribution and its configuration.

## 5. Process Management

### 33. What is a process?

A process is a running instance of a program or application.

### 34. How do you check running processes?

```bash
ps -ef
```

### 35. How do you check processes in real time?

```bash
top
```

### 36. What is PID?

PID stands for Process ID. Every running process has a unique process ID.

### 37. How do you kill a process?

```bash
kill PID
```

If a process does not stop normally:

```bash
kill -9 PID
```

Use `kill -9` only when necessary because it forcefully terminates the process.

### 38. How do you find a process by name?

```bash
pgrep nginx
```

### 39. What is the difference between kill and kill -9?

- `kill PID` normally sends SIGTERM, asking the process to stop gracefully.
- `kill -9 PID` sends SIGKILL and forcefully terminates the process.

## 6. Service Management

### 40. What is systemctl?

`systemctl` is used to manage services on systems that use systemd.

### 41. How do you check service status?

```bash
sudo systemctl status nginx
```

### 42. How do you start, stop and restart a service?

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

### 43. What is the difference between restart and reload?

- Restart stops and starts the service again.
- Reload asks the service to reload its configuration without a full restart, if the service supports it.

### 44. How do you enable a service at boot time?

```bash
sudo systemctl enable nginx
```

### 45. How do you check failed services?

```bash
systemctl --failed
```

## 7. Disk and Memory Management

### 46. How do you check disk usage?

```bash
df -h
```

It displays filesystem disk usage in a human-readable format.

### 47. How do you check directory size?

```bash
du -sh /var/log
```

### 48. How do you check memory usage?

```bash
free -h
```

### 49. What is the difference between df and du?

- `df` shows filesystem-level disk usage.
- `du` shows the disk space used by files and directories.

### 50. How do you check CPU usage?

```bash
top
```

You can also use:

```bash
mpstat
```

If the `sysstat` package is installed.

### 51. What is swap memory?

Swap is disk space used as additional virtual memory when physical RAM is under pressure. It is slower than RAM.

## 8. Networking

### 52. How do you check the IP address?

```bash
ip addr
```

### 53. How do you check network connectivity?

```bash
ping google.com
```

### 54. How do you check open ports?

```bash
ss -tulnp
```

### 55. How do you check DNS resolution?

```bash
nslookup google.com
```

Or:

```bash
dig google.com
```

### 56. What is SSH?

SSH stands for Secure Shell. It is used to securely connect to remote Linux servers.

### 57. What is the default SSH port?

The default SSH port is `22`, unless it has been changed.

### 58. How do you connect to a remote Linux server?

```bash
ssh username@server-ip
```

With a private key:

```bash
ssh -i my-key.pem ec2-user@server-ip
```

## 9. Logs and Text Processing

### 59. Where are Linux logs stored?

Most system logs are stored under:

```bash
/var/log/
```

### 60. How do you check the last 50 lines of a log file?

```bash
tail -n 50 /var/log/messages
```

The log filename depends on the Linux distribution and service.

### 61. How do you monitor logs continuously?

```bash
tail -f /var/log/messages
```

### 62. What is grep?

`grep` searches for a particular word or pattern in a file.

```bash
grep "ERROR" application.log
```

### 63. What is awk?

`awk` is used to process text and extract specific columns or fields.

```bash
awk '{print $1}' file.txt
```

### 64. What is sed?

`sed` is a stream editor used to search, replace, and modify text.

```bash
sed 's/old/new/g' file.txt
```

### 65. What is the difference between head and tail?

- `head` displays the beginning of a file.
- `tail` displays the end of a file.

## 10. Shell Scripting and Cron

### 66. What is shell scripting?

Shell scripting means writing multiple Linux commands in a file and executing them together to automate tasks.

### 67. What is a shebang?

A shebang specifies which interpreter should execute a script.

```bash
#!/bin/bash
```

### 68. How do you execute a shell script?

```bash
chmod +x script.sh
./script.sh
```

Or:

```bash
bash script.sh
```

### 69. What is cron?

Cron is a Linux job scheduler. It runs commands or scripts automatically at scheduled times.

### 70. How do you edit cron jobs?

```bash
crontab -e
```

### 71. How do you list cron jobs?

```bash
crontab -l
```

### 72. Explain a disk usage monitoring script.

"I can write a shell script to check filesystem usage. If disk usage crosses 90%, the script generates a critical alert. If it crosses 75%, it generates a warning. I can schedule the script using cron so that disk usage is checked automatically."

