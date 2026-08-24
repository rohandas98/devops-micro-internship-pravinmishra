# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS (VPC + ALB + ASG + Multi-AZ RDS)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot1.png)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot2.png)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot3A.png)
![Assignment 6 screenshots](screenshots/Assignment5/Screenshot3B.png)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot4A.png)
![Assignment 6 screenshots](screenshots/Assignment5/Screenshot4B.png)

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot5.png)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot6.png)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot7.png)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot8.png)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot9.png)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot10.png)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot11.png)

---

#### Screenshot 12 — A running instance created from the template showing that the application responds on port 80 through a local test or browser using its public IP

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot12.png)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot13.png)

---

#### Screenshot 14 — Target group showing at least one healthy target

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot14.png)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot15A.png)
![Assignment 6 screenshots](screenshots/Assignment5/Screenshot15B.png)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot16.png)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot17.png)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot18.png)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB.

Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance; timestamps are helpful

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot19.png)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot20.png)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot21.png)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![Assignment 6 screenshots](screenshots/Assignment5/Screenshot22.png)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram, which may be hand-drawn, or an AWS console overview showing the components

![Assignment 6 screenshots](screenshots/Assignment5/aws_two_tier_architecture_v2.jpg)

---

### Notes

Summarize the VPC and subnets across the two Availability Zones.

VPC and Subnets across the two Availability Zones

VPC ID & CIDR: The deployment is built inside a custom virtual private cloud named ha-vpc (vpc-06f76bade1ae0ac4e) with an IPv4 CIDR block of 10.0.0.0/16

Availability Zones: To achieve high availability, the infrastructure spans two Availability Zones in the ap-south-1 (Mumbai) region: ap-south-1a (aps1-az1) and ap-south-1b (aps1-az3)

Subnet Layout: The VPC is segmented into four subnets (two public subnets and two private subnets across the AZs)

public-subnet-A: Located in ap-south-1a with CIDR 10.0.1.0/24

public-subnet-B: Located in ap-south-1b with CIDR 10.0.2.0/24

private-subnet-A: Located in ap-south-1a with CIDR 10.0.11.0/24

private-subnet-B: Located in ap-south-1b with CIDR 10.0.12.0/24

Routing & Internet Gateways:
Public Route Table (public-rt / rtb-02d087b42be6f9024): Associated with both Public Subnet A and Public Subnet B
 It directs all outbound traffic (0.0.0.0/0) through an Internet Gateway (igw-04e549db0ef8b446e)

Private Route Table (private-rt / rtb-0d6afe92d3df65996): Associated with Private Subnet A and Private Subnet B
 It routes outbound traffic (0.0.0.0/0) through a NAT Gateway
 to allow private components to safely pull updates.

NAT Gateway (ha-nat-gw / nat-089bfa60c927493c): Deployed within public-subnet-A and is configured with the public Elastic IP 15.252.14.192

Summarize the ALB and Auto Scaling Group setup.

ALB and Auto Scaling Group Setup
Application Load Balancer (ha-alb): An internet-facing load balancer spanning the two public subnets (public-subnet-A and public-subnet-B) across both Availability Zones

DNS Name: ha-alb-1803956100.ap-south-1.elb.amazonaws.com

Security Group (HA-ALB-Security-Group / sg-05c00052a43c2d36): Configured to accept inbound HTTP (Port 80) traffic from the public internet

Listeners & Routing: An HTTP Listener on Port 80 is configured to forward 100% of incoming web traffic to the target group ha-web-tg

Auto Scaling Group (ha-asg): Managed scaling policy that maintains instance redundancy by distributing compute resources across both public subnets

Group Size: Configured with a Desired Capacity of 2, Minimum Capacity of 2, and Maximum Capacity of 4 instances

Launch Template (HA-WEB-Launch-Template): Automatically provisions t3.micro EC2 instances running Linux
 On launch, it executes a User Data script that updates system repositories, installs the Apache (apache2) web server, PHP, and required PHP extensions, and downloads and extracts WordPress directly to /var/www/html/ with proper directory permissions

EC2 Security Group (ha-web-sg / sg-026cd28df7792b383): Allows inbound HTTP (Port 80) traffic routed from the load balancer and SSH (Port 22) for administrator management

Health Checks: Monitors instance stability using both EC2 and ELB health check types with a health check grace period of 300 seconds


Summarize the private Multi-AZ RDS setup.

Private Multi-AZ RDS Setup
Database Instance (ha-db): Managed database instance isolated in the private subnets to complete the secure two-tier pattern

DB Endpoint: ha-db.cvw002g0c4hr.ap-south-1.rds.amazonaws.com running on default MySQL/Aurora port 3306

Public Access: Configured with "Publicly Accessible" set to No, ensuring it is blocked from the public internet

Subnet Group (ha-db-sg): Placed within a DB subnet group containing private-subnet-A (subnet-07ecd883427fd0953) and private-subnet-B (subnet-062c459bae21ead9a)

High Availability: Configured as a Multi-AZ deployment
 The active instance resides in one zone (e.g., ap-south-1b), and automatically replicates data synchronously to a secondary/standby standby database in the other zone to facilitate auto-failover in case of AZ disruption

Database Security Group (ha-db-sg / sg-062b18653dd48a78a): Protects the database layer by only allowing inbound MySQL/Aurora (Port 3306) connections originating from EC2 instances assigned to the ha-web-sg security group.

Summarize the results of both high-availability tests.

Results of Both High-Availability Tests

EC2 Web-Tier Failure & Auto Scaling Recovery Validation:
Scenario: To test self-healing of the web tier, a running web server instance (such as i-0f0db9e1930636fd3 or i-00f48b9af6e0c1138) was terminated

ALB Response: The Application Load Balancer's target group marked the failing instance as Unhealthy (due to failed health checks) and instantly rerouted all traffic away from it to the remaining healthy instance in the opposite Availability Zone

ASG Response: The Auto Scaling Group detected the unhealthy state, marked the instance for replacement (i-049329fa2a617f901 showing as Terminating), and automatically launched a new, healthy EC2 instance (i-09091577602de7f0f in ap-south-1a) to restore the desired capacity of 2

Availability Outcome: The WordPress site remained 100% accessible at the ALB DNS URL (ha-alb-1803956100.ap-south-1.elb.amazonaws.com) throughout the failure and launch process without any user disruption

App-to-Database Multi-AZ Integration Validation:
Scenario: Verified the web application could successfully write to and retrieve data from the isolated Multi-AZ database

Validation Step 1: Reaching the ALB DNS URL for the first time successfully routed the client to the WordPress installation screen
 Completing the admin credentials triggered a successful SQL write to the database engine, creating the core application schemas in ha-db

Validation Step 2: To ensure active data insertion worked, a new test post titled "hi" with the content "twinkle twinkle little stars" was created
 The post was successfully written to the RDS instance and served instantly back to the web UI

Availability Outcome: The Multi-AZ replication kept active data synchronised
 In the event of an AZ disruption, the synchronous standby instance would assume the primary role without data loss, ensuring complete end-to-end resilience of both the compute and data tiers


---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

https://lnkd.in/p/dhPtZh2C

---

#### Screenshot of LinkedIn post

![Assignment 6 screenshots](screenshots/Assignment5/Linked_in_post.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [✅] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [✅] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [✅] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [✅] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [✅] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [✅] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [✅] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [✅] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [✅] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [✅] LinkedIn post published and URL submitted
- [✅] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*