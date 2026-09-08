# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![Week7 screenshots](screenshots/Assignment6/Screenshot1_Architecture.png)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

![Week7 screenshots](screenshots/Assignment6/screenshot2_architecture_notes.png)

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![Week7 screenshots](screenshots/Assignment6/Screenshot3.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![Week7 screenshots](screenshots/Assignment6/Screenshot4.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot4_1.png)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![Week7 screenshots](screenshots/Assignment6/Screenshot5.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![Week7 screenshots](screenshots/Assignment6/Screenshot6.png)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![Week7 screenshots](screenshots/Assignment6/Screenshot7.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![Week7 screenshots](screenshots/Assignment6/Screenshot8.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

![Week7 screenshots](screenshots/Assignment6/Screenshot9.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![Week7 screenshots](screenshots/Assignment6/Screenshot10.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![Week7 screenshots](screenshots/Assignment6/Screenshot11.png)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![Week7 screenshots](screenshots/Assignment6/Screenshot12.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![Week7 screenshots](screenshots/Assignment6/Screenshot13_1.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot13_2.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![Week7 screenshots](screenshots/Assignment6/Screenshot14.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![Week7 screenshots](screenshots/Assignment6/Screenshot15.png)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![Week7 screenshots](screenshots/Assignment6/Screenshot16.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot16_1.png)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![Week7 screenshots](screenshots/Assignment6/Screenshot17.png)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![Week7 screenshots](screenshots/Assignment6/Screenshot18_1.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot18_2.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![Week7 screenshots](screenshots/Assignment6/Screenshot19.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![Week7 screenshots](screenshots/Assignment6/Screenshot20_1.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot20_2.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot20_3.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![Week7 screenshots](screenshots/Assignment6/Screenshot21.png)
![Week7 screenshots](screenshots/Assignment6/Screenshot21_1.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![Week7 screenshots](screenshots/Assignment6/Screenshot22.png)

---

#### Public Endpoint

Paste your public endpoint URL here:

http://52.140.63.7 

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

Architecture Implementation and Deployment Notes

⚬	What Worked: The foundational zero-trust network isolation successfully restricted traffic flow to the intended paths. The Next.js frontend and Node.js backend were successfully daemonized using PM2 and systemd, ensuring they survive server reboots. The Azure NAT Gateway securely handled outbound package downloads for the private Application tier without exposing it to inbound internet traffic. Subnet delegation for the Azure Database for MySQL Flexible Server effectively secured the database inside the VNet without a public endpoint.


Issues Encountered & Resolutions:

⚬	Internal Load Balancer Port Mismatch: During the Internal Load Balancer setup, the load-balancing rules and health probes were mistakenly configured to listen on port 80. However, the Node.js backend application was configured to run and receive requests on port 3001. Fix: The Internal Load Balancer's frontend port, backend port, and TCP health probe were updated to target port 3001, allowing traffic to route correctly to the App VM.
⚬	Nginx Reverse Proxy IP Misconfiguration: API requests from the frontend were failing because the proxy_pass directive in /etc/nginx/sites-available/book-review was pointing to the wrong IP. It was initially set to proxy_pass [http://10.0.2.50:3001/api/](http://10.0.2.50:3001/api/);, but the actual target IP needed to be 10.0.2.5. Fix: The Nginx configuration file was updated with the correct IP address, and the Nginx service was restarted to apply the proxy routing changes.

Understanding [http://10.0.2.5:3001](http://10.0.2.5:3001):

⚬	In this architecture, 10.0.2.5 was provisioned as the static private Frontend IP address for the Internal Azure Load Balancer sitting inside the Book-Review-App-Subnet.
⚬	Port 3001 is the application port where the Node.js backend listens for traffic.
⚬	The complete URI [http://10.0.2.5:3001/api/](http://10.0.2.5:3001/api/) serves as the private entry point for the Application tier. When Nginx (on the Web VM) receives an API request, it forwards it to this Load Balancer address, which then health-checks the backend App VMs and distributes the traffic to them.
⚬	Design Choices (Availability, Security, Secrets, Monitoring, Backup):
⚬	Availability: Both the Web and App tiers utilize Azure Load Balancers with TCP health probes to automatically detect instances and route traffic only to healthy VMs. PM2 process management ensures application persistence if a Node.js process crashes.
⚬	Security: Network Security Groups (NSGs) were applied strictly at the subnet level. The App VM was deployed with no public IP, and the MySQL database was isolated using VNet integration.
⚬	Secrets: Sensitive database credentials and JWT authentication tokens were kept out of the source code by injecting them locally via .env files on the server.
⚬	Monitoring & Backup: Basic infrastructure monitoring relies on Load Balancer health probes and Azure Monitor metrics for VM CPU/Network utilization. The Azure Database for MySQL Flexible Server handles automated backups and data retention natively as a managed service.

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [✅] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [✅] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [✅] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [✅] Task 4: Presentation tier deployed (Screenshots 8–9)
- [✅] Task 5: Application tier deployed privately (Screenshots 10–12)
- [✅] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [✅] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [✅] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
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
