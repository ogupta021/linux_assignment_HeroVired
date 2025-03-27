## Task 1 - System Monitoring & Setup

## Objective
This setup ensures system health, performance monitoring, and capacity planning for a development environment. The goal is to track CPU, memory, disk usage, and identify resource-intensive processes.

---

## Installation and Configuration

### 1. Install Monitoring Tools (`htop` & `nmon`)

sudo apt update
![image](https://github.com/user-attachments/assets/be93de03-9ffe-44cb-b748-13d5eea2e84e)

sudo apt install htop nmon -y
![image](https://github.com/user-attachments/assets/7e610c0d-9c52-49c6-ac09-55a45102c70e)


### 2. Usage
- start htop  
  ![image](https://github.com/user-attachments/assets/43be298c-a0ec-4f8f-8ce6-ffecfec8b2bd)

- start nmon
- ![image](https://github.com/user-attachments/assets/7c474fb2-6cc7-4b88-bc08-93372930bbe2)
- ![image](https://github.com/user-attachments/assets/95e76199-9be3-4928-a9d4-eef3d711db99)
- ![image](https://github.com/user-attachments/assets/c3257b95-969a-4402-900d-57a006a49518)
- ![image](https://github.com/user-attachments/assets/ea79b0b1-23d7-420f-880a-fca5753afa40)



  ```

---
### Check Storage Availability
df -h
![image](https://github.com/user-attachments/assets/bbf00ad4-19dc-477a-b478-363273a32dd0)


### Monitor Specific Directories
du -sh /home/*
![image](https://github.com/user-attachments/assets/671c7b08-6fce-4884-b095-7ab191303676)


### Automate Disk Usage Logging for every 1 hour

echo "0 */1 * * * df -h > /var/log/disk_usage.log" | sudo tee -a /etc/crontab
![image](https://github.com/user-attachments/assets/85129106-daa3-4ed6-8a4a-c275317b4c75)

```

---

## Process Monitoring

### Identify Resource-Intensive Processes
- List top memory-consuming processes:
  ps aux --sort=-%mem | head -10
![image](https://github.com/user-attachments/assets/775a0cd1-edca-46b6-8aa1-0e2680bad4aa)

  ```
- List top CPU-consuming processes:
  ps aux --sort=-%cpu | head -10
  ![image](https://github.com/user-attachments/assets/3a7cc45a-b15b-4066-9482-66f3bc20cb3a)

  ```

### Log Resource Usage for Review

ps aux --sort=-%mem | head -10 >> /var/log/mem_usage.log
![image](https://github.com/user-attachments/assets/509a2d6f-b496-4b13-b913-7fd738ae3701)

ps aux --sort=-%cpu | head -10 >> /var/log/cpu_usage.log
![image](https://github.com/user-attachments/assets/e7d90761-786e-4554-8a26-c5b277ccebf0)


```

---

## System Monitoring Report

### Create a Script (`monitoring_report.sh`)
![image](https://github.com/user-attachments/assets/6c69b679-a7fb-42b1-a2fe-a398bfad44a6)

Edit the script and below lines-
![image](https://github.com/user-attachments/assets/b3f4d55b-168e-443e-a2bc-021bbd89d2b3)


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
```
![image](https://github.com/user-attachments/assets/4af8c413-90e3-4d75-a271-a26bf88a3d28)

##Make it executable 
![image](https://github.com/user-attachments/assets/6f419c15-cdb4-4965-b429-670befb0a54d)


### Schedule the Script to Run Daily at Midnight
echo "0 0 * * * /root/monitoring_report.sh" | sudo tee -a /etc/crontab

![image](https://github.com/user-attachments/assets/5c92b445-c7c2-4e18-9d5d-7702028dd082)


### Check Logs
cat /var/log/system_report.log
![image](https://github.com/user-attachments/assets/2262485a-4f7d-43e7-856b-4093825b802d)

----------------------------------------------------------------------------------------------------------------------------


## Task 2 - User Management and Access Control

### Create User Accounts
sudo useradd -m -s /bin/bash Sarah
![image](https://github.com/user-attachments/assets/85568861-303f-4c7c-9e7f-bb2562e18847)
sudo useradd -m -s /bin/bash Mike
![image](https://github.com/user-attachments/assets/29a4f1a5-8742-4100-9949-8f765aae74a1)


### Set Secure Passwords
sudo passwd Sarah
![image](https://github.com/user-attachments/assets/88318152-f772-4b72-bc09-c404df6d3fe8)

sudo passwd Mike
![image](https://github.com/user-attachments/assets/971d1de4-3372-4fec-b954-9ff6d689259b)


### Set Up Isolated Directories
sudo mkdir -p /home/Sarah/workspace
![image](https://github.com/user-attachments/assets/9a087d21-8281-4542-88bd-d6ebde6bcc16)


sudo mkdir -p /home/mike/workspace
![image](https://github.com/user-attachments/assets/ac5293e6-d2fe-451b-b15a-88e919e086d8)


### Set Permissions to Restrict Access
sudo chown Sarah:Sarah /home/Sarah/workspace
sudo chown Mike:Mike /home/mike/workspace
sudo chmod 700 /home/Sarah/workspace
sudo chmod 700 /home/mike/workspace
![image](https://github.com/user-attachments/assets/c57889a3-750a-4c3f-9380-22a513e15942)

![image](https://github.com/user-attachments/assets/97a38876-42e9-4f0c-9c75-b70c7e900035)
![image](https://github.com/user-attachments/assets/cf5e9fea-af55-4d1e-a859-b40fd07011da)


### Implement Password Policy
![image](https://github.com/user-attachments/assets/84fda0e0-c9b9-4a7a-a08d-9ad80edad6a1)

Edit `/etc/login.defs` and ensure the following parameters are set:
PASS_MAX_DAYS   30
PASS_MIN_DAYS   2
PASS_WARN_AGE   7
![image](https://github.com/user-attachments/assets/63600daa-29a7-4259-a4a6-26d63842a7ec)

Apply the policy:
sudo chage -M 30 -m 2 -W 7 Sarah
sudo chage -M 30 -m 2 -W 7 Mike

![image](https://github.com/user-attachments/assets/ffb54158-bfd8-4802-ac65-5a3862c52712)




