Task 1: System Monitoring Setup
Objective
Configure a monitoring system to ensure system health, performance, and capacity planning.

Step 1: Install and Configure Monitoring Tools
Install htop or nmon for CPU, Memory, and Process Monitoring
sudo apt update
sudo apt install htop nmon -y

Run and Use Monitoring Tools
To start htop:

htop
View real-time CPU, memory, and process usage.

Use F6 to sort by memory or CPU usage.

Press F10 to exit.

To start nmon:

bash
Copy
Edit
nmon
Press c to monitor CPU, m for memory, d for disk, and q to quit.

Step 2: Disk Usage Monitoring
Check Storage Availability
View overall disk space:

bash
Copy
Edit
df -h
Monitor specific directories:

bash
Copy
Edit
du -sh /home/*
Automate Disk Usage Logging
Set up a cron job to log disk usage every 12 hours:

bash
Copy
Edit
echo "0 */12 * * * df -h > /var/log/disk_usage.log" | sudo tee -a /etc/crontab
View logs:

bash
Copy
Edit
cat /var/log/disk_usage.log
Step 3: Process Monitoring to Identify Resource-Intensive Applications
Find Top CPU and Memory Consuming Processes
List top memory-consuming processes:

bash
Copy
Edit
ps aux --sort=-%mem | head -10
List top CPU-consuming processes:

bash
Copy
Edit
ps aux --sort=-%cpu | head -10
Log Resource Usage for Review
Save outputs to a log file:

bash
Copy
Edit
ps aux --sort=-%mem | head -10 >> /var/log/mem_usage.log
ps aux --sort=-%cpu | head -10 >> /var/log/cpu_usage.log
Step 4: Create a Basic Reporting Structure
Generate a System Monitoring Report
Create a script (monitoring_report.sh) to generate a system report:

bash
Copy
Edit
#!/bin/bash

echo "==== System Monitoring Report ====" > /var/log/system_report.log
date >> /var/log/system_report.log
echo "" >> /var/log/system_report.log

echo "== CPU Usage ==" >> /var/log/system_report.log
top -b -n1 | head -10 >> /var/log/system_report.log
echo "" >> /var/log/system_report.log

echo "== Memory Usage ==" >> /var/log/system_report.log
free -h >> /var/log/system_report.log
echo "" >> /var/log/system_report.log

echo "== Disk Usage ==" >> /var/log/system_report.log
df -h >> /var/log/system_report.log
echo "" >> /var/log/system_report.log

echo "== Top 10 Processes by CPU Usage ==" >> /var/log/system_report.log
ps aux --sort=-%cpu | head -10 >> /var/log/system_report.log
echo "" >> /var/log/system_report.log

echo "== Top 10 Processes by Memory Usage ==" >> /var/log/system_report.log
ps aux --sort=-%mem | head -10 >> /var/log/system_report.log
echo "==== End of Report ====" >> /var/log/system_report.log
Schedule the Report Generation
Make the script executable:

bash
Copy
Edit
sudo chmod +x /root/monitoring_report.sh
Schedule it to run daily at midnight:

bash
Copy
Edit
echo "0 0 * * * /root/monitoring_report.sh" | sudo tee -a /etc/crontab
Check Logs
View the system report:

bash
Copy
Edit
cat /var/log/system_report.log
