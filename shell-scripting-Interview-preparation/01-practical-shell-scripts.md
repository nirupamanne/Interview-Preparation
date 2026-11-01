# Practical Shell Scripting – Real-Time DevOps Examples

This document contains practical Bash scripts commonly used in Linux and DevOps environments.

Each example includes:
- Script
- How to execute
- Expected output
- Real-time use case

---

## 1. Check Disk Space Usage

**Scenario:** Monitor disk usage and display a warning when usage exceeds 75% and a critical alert when it exceeds 90%.

```bash
#!/bin/bash

THRESHOLD_WARNING=75
THRESHOLD_CRITICAL=90

df -P | awk 'NR>1 {print $5, $6}' | while read usage mount
do
    usage=${usage%\%}

    if [ "$usage" -ge "$THRESHOLD_CRITICAL" ]; then
        echo "CRITICAL: Disk usage on $mount is ${usage}%"
    elif [ "$usage" -ge "$THRESHOLD_WARNING" ]; then
        echo "WARNING: Disk usage on $mount is ${usage}%"
    else
        echo "OK: Disk usage on $mount is ${usage}%"
    fi
done
```

**Execute:**
```bash
chmod +x disk-check.sh
./disk-check.sh
```

**Use case:** Helps administrators identify disks running out of space before applications are affected.

## How to Explain in an Interview

"I have written a Bash script to monitor disk space usage on a Linux server.

First, I use the `df` command to check disk usage. Then I use `awk` to extract the disk usage percentage and mount point.

I configured two thresholds: 75% for warning and 90% for critical.

If disk usage goes above 75%, the script displays a warning message. If it reaches 90% or more, it displays a critical message."

*This helps the DevOps team identify disk space issues early and take action before applications are affected.*
---

## 2. Check Whether a Service Is Running

**Scenario:** Check whether the Nginx service is running.

```bash
#!/bin/bash

SERVICE="nginx"

if systemctl is-active --quiet "$SERVICE"; then
    echo "$SERVICE is running"
else
    echo "$SERVICE is not running"
fi
```

**Execute:**
```bash
chmod +x service-check.sh
./service-check.sh
```

**Use case:** Used to verify application and system services.

---

## 3. Start a Service If It Is Stopped

**Scenario:** If Nginx is stopped, start it automatically.

```bash
#!/bin/bash

SERVICE="nginx"

if systemctl is-active --quiet "$SERVICE"; then
    echo "$SERVICE is already running"
else
    echo "$SERVICE is stopped. Starting..."
    sudo systemctl start "$SERVICE"

    if systemctl is-active --quiet "$SERVICE"; then
        echo "$SERVICE started successfully"
    else
        echo "Failed to start $SERVICE"
    fi
fi
```

**Execute:**
```bash
chmod +x service-start.sh
./service-start.sh
```

**Use case:** Basic service recovery automation. The script may require sudo permissions.

---

## 4. Check File Exists or Not

**Scenario:** Check whether an application configuration file exists.

```bash
#!/bin/bash

FILE="/etc/hosts"

if [ -f "$FILE" ]; then
    echo "$FILE exists"
else
    echo "$FILE does not exist"
fi
```

**Execute:**
```bash
chmod +x file-check.sh
./file-check.sh
```

**Use case:** Verify configuration files before starting an application.

---

## 5. Create a Backup of a Directory

**Scenario:** Take a timestamped backup of an application directory.

```bash
#!/bin/bash

SOURCE="/opt/app"
DEST="/backup"

DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$DEST"

if [ -d "$SOURCE" ]; then
    tar -czf "$DEST/app-backup-$DATE.tar.gz" -C "$(dirname "$SOURCE")" "$(basename "$SOURCE")"
    echo "Backup completed successfully"
else
    echo "Source directory does not exist"
    exit 1
fi
```

**Execute:**
```bash
chmod +x backup.sh
sudo ./backup.sh
```

**Use case:** Take backups before deployments or maintenance activities.

---

## 6. Find Files Larger Than 100 MB

**Scenario:** Identify large files consuming disk space.

```bash
#!/bin/bash

DIRECTORY="/var"

echo "Finding files larger than 100 MB..."

find "$DIRECTORY" -type f -size +100M -exec ls -lh {} \;
```

**Execute:**
```bash
chmod +x large-files.sh
sudo ./large-files.sh
```

**Use case:** Troubleshoot disk space issues.

---

## 7. Check Memory Usage

**Scenario:** Display total, used, and available memory.

```bash
#!/bin/bash

free -h
```

**Execute:**
```bash
chmod +x memory-check.sh
./memory-check.sh
```

**Use case:** Monitor memory consumption and investigate performance issues.

---

## 8. Check CPU Usage

**Scenario:** Display CPU usage information.

```bash
#!/bin/bash

top -bn1 | head -n 5
```

**Execute:**
```bash
chmod +x cpu-check.sh
./cpu-check.sh
```

**Use case:** Initial investigation when a Linux server is running slowly.

---

## 9. Check Whether a Process Is Running

**Scenario:** Check whether the Java process is running.

```bash
#!/bin/bash

PROCESS="java"

if pgrep -x "$PROCESS" > /dev/null; then
    echo "$PROCESS process is running"
else
    echo "$PROCESS process is not running"
fi
```

**Execute:**
```bash
chmod +x process-check.sh
./process-check.sh
```

**Use case:** Verify whether an application process is running.

---

## 10. Monitor Log Files for Errors

**Scenario:** Find error messages in an application log.

```bash
#!/bin/bash

LOG_FILE="/var/log/messages"

if [ -f "$LOG_FILE" ]; then
    grep -i "error" "$LOG_FILE" | tail -n 20
else
    echo "Log file not found"
fi
```

**Execute:**
```bash
chmod +x log-check.sh
sudo ./log-check.sh
```

**Use case:** Troubleshoot application or system errors using logs.

---

## 11. Check Server Uptime

**Scenario:** Display how long the Linux server has been running.

```bash
#!/bin/bash

echo "Server uptime:"
uptime
```

**Execute:**
```bash
chmod +x uptime.sh
./uptime.sh
```

**Use case:** Check server availability and recent restarts.

---

## 12. Check and Create a User

**Scenario:** Create a Linux user only if the user does not already exist.

```bash
#!/bin/bash

USER="deploy"

if id "$USER" > /dev/null 2>&1; then
    echo "User $USER already exists"
else
    sudo useradd "$USER"

    if [ $? -eq 0 ]; then
        echo "User $USER created successfully"
    else
        echo "User creation failed"
    fi
fi
```

**Execute:**
```bash
chmod +x user-check.sh
./user-check.sh
```

**Use case:** Basic user-management automation. Requires appropriate sudo permissions.

---

## 13. Delete Old Log Files

**Scenario:** Find and delete `.log` files older than 7 days from an application log directory.

```bash
#!/bin/bash

LOG_DIR="/var/log/myapp"

if [ -d "$LOG_DIR" ]; then
    find "$LOG_DIR" -type f -name "*.log" -mtime +7 -print -delete
    echo "Old log cleanup completed"
else
    echo "Log directory does not exist"
fi
```

**Execute:**
```bash
chmod +x log-cleanup.sh
sudo ./log-cleanup.sh
```

**Use case:** Log cleanup to prevent unnecessary disk consumption.

**Note:** Check the files using `find "$LOG_DIR" -type f -name "*.log" -mtime +7 -print` before enabling deletion.

---

## 14. Check Website HTTP Status

**Scenario:** Check whether a website is responding successfully.

```bash
#!/bin/bash

URL="https://www.google.com"

STATUS=$(curl -s -o /dev/null -w "%{http_code}" "$URL")

if [ "$STATUS" -eq 200 ]; then
    echo "Website is accessible"
else
    echo "Website returned HTTP status: $STATUS"
fi
```

**Execute:**
```bash
chmod +x website-check.sh
./website-check.sh
```

**Use case:** Basic website availability checks. A production health check should also consider timeouts and expected application-specific responses.

---

## 15. Schedule a Script Using Cron

**Scenario:** Run a disk-space monitoring script every 5 minutes.

Open the cron editor:

```bash
crontab -e
```

Add:

```cron
*/5 * * * * /home/ec2-user/disk-check.sh
```

Check cron jobs:

```bash
crontab -l
```

**Use case:** Automate recurring monitoring tasks without manually executing scripts.

---

## Common Commands to Remember

| Command | Purpose |
|---|---|
| `chmod +x script.sh` | Make script executable |
| `./script.sh` | Execute script |
| `bash -x script.sh` | Debug script |
| `bash -n script.sh` | Check syntax |
| `echo $?` | Check previous command exit status |
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `systemctl status nginx` | Check service status |
| `df -h` | Check disk usage |
| `free -h` | Check memory |
| `ps aux` | List processes |
| `grep -i error file.log` | Search errors in logs |

---


