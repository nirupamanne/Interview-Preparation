# Linux Commands – Quick Revision for DevOps Interviews

A quick reference of frequently used Linux commands, their purpose, and examples.

## 1. Basic Commands

| Command | Purpose |
|---|---|
| `pwd` | Current working directory |
| `whoami` | Current logged-in user |
| `hostname` | Server hostname |
| `date` | Current date and time |
| `uname -a` | System information |
| `cat /etc/os-release` | Linux version |
| `clear` | Clear terminal |
| `history` | Previously executed commands |
| `man ls` | Command manual |

## 2. File and Directory Commands

| Command | Purpose |
|---|---|
| `ls` | List files |
| `ls -l` | Detailed file listing |
| `ls -la` | Include hidden files |
| `mkdir test` | Create directory |
| `mkdir -p a/b/c` | Create nested directories |
| `touch file.txt` | Create empty file |
| `cat file.txt` | Display file content |
| `cp file.txt backup.txt` | Copy file |
| `mv file.txt /tmp/` | Move file |
| `mv old.txt new.txt` | Rename file |
| `rm file.txt` | Delete file |
| `rm -r directory` | Delete directory and contents |
| `rmdir directory` | Delete empty directory |
| `find /home -name "*.txt"` | Find files |
| `file file.txt` | Identify file type |

## 3. File Permissions and Ownership

| Command | Purpose |
|---|---|
| `ls -l` | Check permissions |
| `chmod 755 file.sh` | Change permissions |
| `chmod 700 file.sh` | Owner-only access |
| `chmod 644 file.txt` | Owner read/write; others read |
| `chown alice file.txt` | Change owner |
| `chown alice:devops file.txt` | Change owner and group |
| `chown -R alice:devops directory` | Change ownership recursively |
| `chgrp devops file.txt` | Change group |
| `umask` | Check default permission mask |

**Permission values:**

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |
| `rwx` | 7 |
| `rw-` | 6 |
| `r-x` | 5 |
| `r--` | 4 |

## 4. User and Group Management

| Command | Purpose |
|---|---|
| `useradd alice` | Create user |
| `passwd alice` | Set user password |
| `useradd -m alice` | Create user with home directory |
| `id alice` | Check user ID and groups |
| `who` | Show logged-in users |
| `su - alice` | Switch user |
| `groupadd devops` | Create group |
| `usermod -aG devops alice` | Add user to supplementary group |
| `groups alice` | Check user groups |
| `userdel alice` | Delete user |
| `userdel -r alice` | Delete user and home directory |
| `cat /etc/passwd` | View user account information |
| `cat /etc/group` | View group information |

## 5. Process Management

| Command | Purpose |
|---|---|
| `ps -ef` | List
