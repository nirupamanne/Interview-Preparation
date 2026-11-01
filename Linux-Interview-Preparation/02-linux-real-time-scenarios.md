# Linux Real-Time Scenarios and Experience-Based Interview Questions

This document contains practical Linux troubleshooting scenarios and experience-based interview questions for DevOps Engineer interviews.

The answers are written in simple language so they are easy to understand and explain during interviews.

# Part 1: Real-Time Linux Troubleshooting Scenarios

## Scenario 1: Linux server is running slowly. How do you troubleshoot?

**Interview answer:**

"First, I check the CPU and memory usage using `top` and `free -h`. Then I check disk usage using `df -h`. I identify whether any process is consuming high resources. I also check application and system logs to understand the root cause."

**Commands:**

```bash
# Check CPU and memory usage
top

# Check memory usage
free -h

# Check disk usage
df -h

# Check running processes
ps -ef

# Find processes consuming high CPU
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu
```

**Troubleshooting steps:**

1. Check CPU utilisation.
2. Check available memory.
3. Check disk space.
4. Identify processes consuming high resources.
5. Check application and system logs.
6. Identify the root cause and take action.

## Scenario 2: Disk usage has reached 95%. What will you do?

**Interview answer:**

"First, I identify which filesystem is full using `df -h`. Then I use `du` to find large directories. I check old logs, temporary files, and unnecessary data. After confirming with the team, I clean up or archive files safely."

**Commands:**

```bash
# Check filesystem usage
df -h

# Check directory sizes
du -sh /var/log/*

# Find files larger than 100 MB
find /var/log -type f -size +100M

# Check the largest files in a directory
du -ah /var/log | sort -rh | head -10
```

**Troubleshooting steps:**

1. Identify the full filesystem using `df -h`.
2. Find which directory is consuming more space.
3. Identify large log files and unnecessary files.
4. Check whether logs can be rotated or archived.
5. Clean up only after confirming it is safe.
6. Run `df -h` again to verify the available space.

## Scenario 3: Application is not accessible. How do you troubleshoot?

**Interview answer:**

"First, I check whether the application service is running. Then I check application logs, listening ports, server connectivity, and firewall rules. Based on the error, I identify whether the issue is with the application, server, or network."

**Commands:**

```bash
# Check application service
systemctl status nginx

# Check listening ports
ss -tulnp

# Check local application response
curl -I http://localhost

# Check application error logs
tail -f /var/log/nginx/error.log
```

**Troubleshooting steps:**

1. Check whether the application service is running.
2. Check whether the application is listening on the expected port.
3. Test the application locally using curl.
4. Check application logs for errors.
5. Verify firewall and network connectivity.
6. Restart the service only after understanding the issue.

## Scenario 4: SSH connection is failing. What will you check?

**Interview answer:**

"I verify the server IP, username, and SSH port. Then I check whether the SSH service is running, whether port 22 is reachable, and whether the security group or firewall allows my IP. I also verify the private key and its permissions."

**Commands:**

```bash
# Check SSH service
sudo systemctl status sshd

# Check whether port 22 is listening
sudo ss -tulnp | grep :22

# Connect with verbose debugging
ssh -v username@server-ip

# Check private key permissions
chmod 400 my-key.pem
```

**Troubleshooting steps:**

1. Verify the server IP and username.
2. Verify the SSH port.
3. Check whether the SSH service is running.
4. Check firewall and cloud security group rules.
5. Verify the private key and its permissions.
6. Check the SSH verbose output to identify the connection error.

## Scenario 5: A service has stopped. How will you troubleshoot?

**Interview answer:**

"I check the service status and review its logs to understand why it stopped. I check configuration, resource availability, and dependencies. After identifying the issue, I fix it and restart the service."

**Commands:**

```bash
# Check service status
sudo systemctl status nginx

# Check recent service logs
sudo journalctl -u nginx -n 50

# Check service logs from the current boot
sudo journalctl -u nginx -b

# Restart the service after fixing the issue
sudo systemctl restart nginx

# Verify service status
sudo systemctl status nginx
```

**Troubleshooting steps:**

1. Check service status.
2. Review service logs.
3. Check configuration files.
4. Verify dependencies and available resources.
5. Fix the identified issue.
6. Restart the service and verify that it is running.

## Scenario 6: A process is consuming high CPU. What will you do?

**Interview answer:**

"I use `top` to identify the process consuming high CPU. I check the process ID and investigate what application is running. I don't kill the process immediately. First, I understand the impact and coordinate with the application team."

**Commands:**

```bash
# Monitor processes
top

# Sort processes by CPU usage
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu

# Find a process by name
pgrep nginx

# Check details of a process
ps -p PID -f
```

**Troubleshooting steps:**

1. Identify the process consuming high CPU.
2. Note its PID.
3. Understand which application or service owns the process.
4. Check application logs and resource usage.
5. Coordinate with the relevant team.
6. Restart or terminate the process only after approval and impact assessment, if required.

## Scenario 7: A user cannot access a file. How will you troubleshoot?

**Interview answer:**

"I check the file permissions, ownership, and group membership. I verify whether the user has the required read, write, or execute permissions. If needed, I update permissions or ownership according to the access requirement."

**Commands:**

```bash
# Check file permissions and ownership
ls -l file.txt

# Check user and group membership
id username

# Check permissions of each directory in the path
namei -l /path/to/file

# Change ownership if required
sudo chown username:groupname file.txt

# Change permissions if required
sudo chmod 640 file.txt
```

**Troubleshooting steps:**

1. Check file ownership.
2. Check file permissions.
3. Verify the user's group membership.
4. Check execute permission on parent directories.
5. Apply the required permissions according to the access policy.
6. Ask the user to verify access.

## Scenario 8: A server cannot resolve a domain name. What will you check?

**Interview answer:**

"I check whether the server has network connectivity. Then I verify DNS resolution using `nslookup` or `dig`. I also check the DNS configuration and resolver settings."

**Commands:**

```bash
# Check connectivity using an IP address
ping -c 4 8.8.8.8

# Check DNS resolution
nslookup google.com

# Check DNS details
dig google.com

# Check DNS resolver configuration
cat /etc/resolv.conf
```

**Troubleshooting steps:**

1. Check basic network connectivity.
2. Test whether an external IP is reachable.
3. Check DNS resolution.
4. Verify the DNS resolver configuration.
5. Check whether the configured DNS server is reachable.
6. Coordinate with the network team if DNS connectivity is failing.

## Scenario 9: The application is running, but users cannot access it.

**Interview answer:**

"I check whether the application is listening on the expected port. Then I verify the local application response, firewall rules, security group settings, and network connectivity. I also check whether the load balancer is forwarding traffic correctly, if one is used."

**Commands:**

```bash
# Check application status
systemctl status nginx

# Check listening ports
ss -tulnp

# Test application locally
curl -I http://localhost

# Check local firewall status on firewalld systems
sudo firewall-cmd --state
```

**Troubleshooting steps:**

1. Check application service status.
2. Verify the application listening port.
3. Test the application locally.
4. Check server firewall rules.
5. Check cloud security group and network ACL rules, where applicable.
6. Verify load balancer health checks and target status, if applicable.

## Scenario 10: How do you troubleshoot after a server reboot?

**Interview answer:**

"I check whether the server is up and reachable. Then I verify critical services, disk space, memory, and system logs. I also check whether required services are enabled to start automatically after reboot."

**Commands:**

```bash
# Check uptime
uptime

# Check failed services
systemctl --failed

# Check application service
systemctl status nginx

# Check errors from the current boot
journalctl -b -p err

# Check whether a service is enabled at boot
systemctl is-enabled nginx
```

**Troubleshooting steps:**

1. Verify that the server is reachable.
2. Check system uptime.
3. Check failed services.
4. Verify important application services.
5. Check system logs for boot errors.
6. Enable required services at boot if appropriate.

---

# Part 2: Experience-Based Linux Interview Questions

These answers are examples for a candidate with around 5 years of overall IT experience. Adapt them to your actual responsibilities and projects.

## 1. Tell me about your Linux experience.

**Sample answer:**

"I have around 5 years of overall IT experience, including experience working in DevOps. I have worked with Linux servers for day-to-day administration and troubleshooting activities.

I am comfortable with file and directory management, user and group management, permissions, process management, service management, disk and memory checks, networking, SSH, and log analysis.

I use commands such as `grep`, `awk`, `sed`, `find`, `df`, `du`, `top`, and `systemctl` for troubleshooting.

I have also worked on shell scripting to automate repetitive tasks, such as disk usage monitoring and alerting. Linux is an important part of DevOps because many application servers and cloud workloads run on Linux."

## 2. What are your day-to-day Linux activities?

**Sample answer:**

"My regular activities include checking server health, monitoring CPU, memory, and disk usage, checking application services, analysing logs, managing users and permissions, and troubleshooting connectivity issues.

Whenever an issue occurs, I first check the system status and logs, identify the root cause, and then take the required action."

## 3. Have you worked on Linux automation?

**Sample answer:**

"Yes. I have worked on basic shell scripting to automate repetitive tasks. For example, I have worked on a disk usage monitoring script. It checks filesystem usage and generates alerts when the usage crosses defined thresholds. I can schedule the script using cron."

## 4. How do you approach production issues?

**Sample answer:**

"Whenever a production issue occurs, I first understand the impact and check whether the server or application is available. I check CPU, memory, disk, services, network connectivity, and logs. I try to identify the root cause before making any changes. If the issue requires application or network team support, I coordinate with them and keep the team updated."

## 5. How do you handle a critical issue under pressure?

**Sample answer:**

"I stay calm and first understand the impact of the issue. I prioritise the problem based on business impact, check the available logs and monitoring information, and work on identifying the root cause. I communicate with the relevant teams and avoid making changes without understanding their impact."

## 6. How do you monitor Linux servers?

**Sample answer:**

"I monitor CPU, memory, disk usage, running processes, and application services. I use Linux commands such as `top`, `free -h`, `df -h`, and `systemctl`. I can also use monitoring tools such as Grafana to review server metrics and identify resource-related issues."

## 7. How do you identify the root cause of a Linux issue?

**Sample answer:**

"First, I understand the issue and its impact. Then I check system resources, running processes, service status, network connectivity, and application logs. I compare the available information with the expected behaviour and narrow down the possible cause. After applying the fix, I verify that the issue is resolved."

## 8. What do you do if you cannot resolve a Linux issue?

**Sample answer:**

"I collect the relevant information, including error messages, logs, service status, and the troubleshooting steps already performed. Then I coordinate with the appropriate team, such as the application, network, or infrastructure team. I share my findings so that we can resolve the issue faster."

---


