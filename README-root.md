# AWS Cloud FinOps & Scalability

A collection of practical AWS projects focused on two complementary cloud engineering goals: **cost-conscious resource management (FinOps)** and **workload-driven scalability**.

This repository contains two independent project folders. Each has its own implementation details, evidence screenshots, and README.

## Projects

### 1. Cloud FinOps — EBS Resource Cleanup Automation

Automates the identification of eligible, tagged EBS resources for cleanup using AWS Lambda, Boto3, IAM, EventBridge, and CloudWatch Logs.

- Scans for EBS volumes and snapshots matching defined cleanup criteria.
- Uses resource tags and age/state checks to identify cleanup candidates.
- Validated in **dry-run mode**, which logs candidates without deleting resources.
- Demonstrates a cautious serverless approach to reducing unmanaged or unnecessary storage.

[Open the FinOps project](./Cloud%20FinOps/)

### 2. EC2 Auto Scaling — Workload-Based Scalability

Configures an EC2 Auto Scaling Group with a CloudWatch-backed target tracking policy based on average CPU utilization.

- Uses a **50% average CPU target** to guide capacity adjustment.
- Demonstrated automatic scale-out from **1 to 2 EC2 instances** when CPU rose to approximately **80.97%**.
- Both instances reached **3/3 status checks passed** after scale-out.
- Scale-in evidence is planned for the next validation step.

[Open the Auto Scaling project](./scalability/)

## Repository structure

```text
.
├── README.md
├── finops-ebs-cleanup/
│   ├── README.md
│   └── screenshots/
└── ec2-auto-scaling/
    ├── README.md
    └── screenshots/
```

> Folder names are illustrative. Update the links and structure above to match the actual directory names in your repository.

## Skills demonstrated

- AWS EC2 and Auto Scaling
- Amazon CloudWatch metrics and logs
- AWS Lambda and Boto3
- Amazon EBS resource lifecycle awareness
- IAM execution roles and permissions
- EventBridge scheduling
- Cloud cost awareness and elastic capacity management
- Validation through AWS console evidence and logs

## Project status and safety

The EBS cleanup project has been tested in dry-run mode; do not describe it as deleting resources automatically unless that behavior is intentionally enabled and separately validated. The Auto Scaling project has demonstrated scale-out; scale-in screenshots and final validation will be added as that test is completed.

No specific cost savings percentage is claimed because cost reduction has not been measured.
