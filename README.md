🖥️ Resilient Web Server — Auto-Healing & Auto-Scaling on AWS
📘 Project Description

This project demonstrates the deployment of a Resilient Web Server architecture on AWS that automatically heals failed instances and scales up or down based on demand. The setup ensures high availability, fault tolerance, and elastic scalability using AWS Auto Scaling Groups and Load Balancer.

🚀 Objective

To design and deploy a self-healing and scalable web infrastructure that maintains performance and uptime automatically — without manual intervention.

⚙️ AWS Services Used

EC2 (Elastic Compute Cloud) – To host the web application
Auto Scaling Group (ASG) – Automatically adds/removes instances
Elastic Load Balancer (ELB) – Distributes traffic across healthy instances
Launch Template – Standard configuration for EC2 instances
CloudWatch Alarms – Monitors performance metrics
SNS (Simple Notification Service) – Sends scaling or health alerts
IAM Roles – Manage permissions securely

🧩 Architecture Overview

A Launch Template defines the EC2 instance setup (AMI, instance type, key pair, security group, user data, etc.).
An Auto Scaling Group (ASG) launches multiple EC2 instances across availability zones.
A Load Balancer (ELB) distributes traffic evenly across instances.
CloudWatch Alarms monitor CPU utilization and trigger scaling actions.
SNS Topic sends notifications during scaling events or failures.

🧰 Prerequisites

AWS Account
Key Pair (for SSH access)
Basic knowledge of EC2, Security Groups, and IAM
Web browser access to AWS Management Console

🪜 Step-by-Step Setup
1️⃣ Create a Launch Template

Go to EC2 → Launch Templates → Create launch template
Enter:
Name: ResilientWebServerTemplate
AMI: Amazon Linux 2
Instance type: t2.micro
Key pair: Select your key pair
Security group: Allow ports 22 (SSH), 80 (HTTP)
Under User Data, paste:

#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from Resilient Web Server! Auto-healing & Scaling Active 🚀</h1>" > /var/www/html/index.html

Create the launch template.

2️⃣ Create an Auto Scaling Group (ASG)

Go to EC2 → Auto Scaling Groups → Create Auto Scaling group
Name: ResilientWebServer-ASG
Choose the launch template you created.
Select at least 2 Availability Zones (e.g., us-east-1a, us-east-1b)
Attach to a Load Balancer (create new or existing):
Type: Application Load Balancer
Target group: Create new (HTTP:80)
Configure scaling policies:
Desired capacity: 2
Min capacity: 1
Max capacity: 3
Add scaling policies:
Scale out: If average CPU ≥ 45%
Scale in: If average CPU ≤ 30%
Enable SNS notifications (optional) for scaling events.
Review and create the ASG.

3️⃣ Verify Setup

Go to Load Balancer → DNS name, copy it, and open in browser.
You should see:

Hello from Resilient Web Server! Auto-healing & Scaling Active 🚀


Terminate one instance manually → ASG automatically launches a new one.

Simulate high load → ASG scales out new instances.

4️⃣ Test via SSH
chmod 400 your-key.pem
ssh -i "your-key.pem" ec2-user@<Public-IP>

To verify web server:

systemctl status httpd
cat /var/www/html/index.html

🧪 Validation Checklist

✅ Website accessible through Load Balancer DNS
✅ Instance automatically replaced when terminated
✅ Scaling triggered under high CPU usage
✅ Notifications received (if SNS enabled)

📂 Repository Structure
resilient-autohealing-webserver/
│
├── README.md
├── architecture-diagram.png   # Optional: Add diagram
└── scripts/
    └── user-data.sh           # EC2 startup script

📊 Outcome

A fully automated, self-healing, and scalable web server setup capable of handling traffic spikes and instance failures without downtime.

🧠 Key Learnings

Implementation of Auto Scaling Groups (ASG)

Integration of Load Balancer with ASG

Configuring CloudWatch Alarms and SNS Notifications

Designing Fault-tolerant web architecture on AWS

🏷️ Author
Apurva Kadam
AWS | Cloud Computing | Automation Enthusiast
