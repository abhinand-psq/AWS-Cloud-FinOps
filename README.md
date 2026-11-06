# AWS Cloud, Linux Administration & DevOps Projects

A collection of hands-on cloud and Linux engineering projects focused on **AWS infrastructure, cloud cost optimization, scalability, Linux administration, troubleshooting, and automation**.

The repository demonstrates practical implementation, troubleshooting, automation, and validation through project documentation and screenshots.

## Projects

### 1. Cloud FinOps — EBS Resource Cleanup Automation

Automates the identification of eligible, tagged EBS resources for cleanup using AWS Lambda, Boto3, IAM, EventBridge, and CloudWatch Logs.

- Scans EBS volumes and snapshots matching defined cleanup criteria.
- Uses resource tags and age/state checks to identify cleanup candidates.
- Validated in **dry-run mode**, logging candidates without deleting resources.
- Demonstrates serverless automation for identifying potentially unnecessary storage resources.

[Open the FinOps project](./Cloud%20FinOps/)

### 2. EC2 Auto Scaling — Workload-Based Scalability

Configures an EC2 Auto Scaling Group with a CloudWatch-backed target tracking policy based on CPU utilization.

- Uses a **50% average CPU utilization target** to guide capacity adjustment.
- Demonstrated automatic scale-out from **1 to 2 EC2 instances** during increased CPU utilization.
- Observed CPU utilization reaching approximately **80.97%** during testing.
- Verified that instances reached **3/3 status checks passed** after scale-out.
- Includes scale-in validation and infrastructure evidence.

[Open the Auto Scaling project](./scalability/)

### 3. Linux Administration & Troubleshooting

Hands-on Linux administration and troubleshooting performed on an Ubuntu EC2 instance running a Node.js application behind NGINX.

- Managed Linux users, groups, directory ownership, and permissions.
- Configured systemd service management for Node.js.
- Troubleshot systemd startup errors and service failures.
- Investigated NGINX 502 Bad Gateway errors using application and NGINX logs.
- Examined application listening addresses and service connectivity.
- Investigated CPU and memory usage using Linux process and system tools.
- Validated recovery after service and resource-related issues.

[Linux Administration](./Linux-administration/)

[Linux & Application Troubleshooting](./troubleshooting/)

### 4. Bash Automation & Cron

Developed Bash scripts to automate application health checks and system resource monitoring on Ubuntu.

- Automated HTTP health checks using `curl`.
- Monitored CPU, memory, and disk utilization.
- Scheduled scripts to execute every five minutes using Cron.
- Recorded timestamped results in Linux log files.
- Verified scheduled execution and generated logs.

[Open the Bash Automation project](./automation/)

---

## Repository Structure

```text
.
├── README.md
│
├── Cloud FinOps/
│   ├── README.md
│   └── screenshots/
│
├── scalability/
│   ├── README.md
│   └── screenshots/
├── linux/
│   ├── README.md
│   └── screenshots/
│
├── troubleshooting/
│   ├── README.md
│   └── screenshots/

└── automation/
        ├── README.md
        ├── screenshots/
        └── scripts/
            ├── app-health-check.sh
            └── resource-monitor.sh            
```    

## Skills Demonstrated

**AWS & Cloud Infrastructure**
- Amazon EC2
- EC2 Auto Scaling Groups
- Launch Templates
- Amazon CloudWatch
- AWS Lambda and Boto3
- Amazon EBS
- IAM roles and permissions
- EventBridge

**Linux Administration**
- Ubuntu Linux
- User and group management
- File permissions and ownership
- systemd service management
- Linux process and resource investigation
- `journalctl` and log analysis

**Automation & Troubleshooting**
- Bash scripting
- Cron scheduling
- Application health checks
- CPU, memory, and disk monitoring
- NGINX reverse proxy troubleshooting
- Node.js service troubleshooting

## Project Validation & Safety

- AWS EBS cleanup automation was validated in **dry-run mode**; resource deletion was not performed.
- EC2 Auto Scaling behavior was validated through CPU stress testing and AWS console evidence.
- Linux service and permission configurations were validated through practical testing.
- Bash automation was verified through Cron schedules and timestamped log output.
- No specific cloud cost savings percentage is claimed because cost reduction has not been measured.
