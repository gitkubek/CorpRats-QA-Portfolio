# CorpRats - QA Portfolio Project (Simulating GameDev Workflow)

# Project Goal

This repository is part of a comprehensive portfolio project designed to showcase professional Quality Assurance (QA) skills within a simulated, realistic Game Development workflow utilizing industry-standard tools and practices. The goal is to demonstrate capabilities in test planning, execution, automation, CI/CD integration, infrastructure management, and process documentation, aiming for a high standard of technical proficiency.

**Current Status (As of May 5, 2025):**
* Core AWS infrastructure (VPC, EC2, Networking, Security) is deployed and stable.
* Self-hosted Perforce (P4D) and PostgreSQL servers are running on EC2.
* WireGuard VPN provides secure, split-tunnel access for the team.
* Initial team members have been onboarded (VPN & Perforce access).
* Automated weekly backups for EC2 instances are configured via AWS Backup.
* Basic infrastructure monitoring (CPU, Status Checks, Disk, RAM) with CloudWatch Alarms is active.
* This public Git repository is set up as a mirror for non-binary project files synced from the primary Perforce depot.
* Core QA process tools (JIRA, TestRail) and CI/CD (Jenkins) are planned but *not yet implemented*.
* Initial UE5 project skeleton exists in Perforce.

---

# Architecture Overview

The project utilizes a secure cloud infrastructure hosted on AWS (eu-north-1 region):

* **VPC:** A standard AWS VPC (`172.31.0.0/16`) with distinct public and private subnets.
* **CorpRatsServer (Private Subnet):** An EC2 instance (`t3.micro`, Ubuntu 22.04, AMI: `ami-087362bfb28866676`) hosting the primary Perforce server (P4D) and the PostgreSQL database. It has no public IP and accesses the internet via NAT.
* **CorpRatsVPN (Public Subnet):** An EC2 instance (`t3.micro`, Ubuntu 22.04, Standard Base AMI) acting as both:
    * A **WireGuard VPN Gateway** providing secure, split-tunnel access to the VPC for developers.
    * A **NAT Gateway** for the `CorpRatsServer`.
    * Protected by an Elastic IP (`16.170.22.220`) and Security Groups.
* **Access Control:** All access to `CorpRatsServer` requires an active WireGuard VPN connection (`10.66.66.0/24` subnet). Security Groups further restrict traffic based on source IP and port.
* **Backup:** Automated weekly AWS Backup for both EC2 instances (35-day warm retention). Historical manual backups may exist on instances.
* **Monitoring:** Basic AWS CloudWatch alarms configured for CPU, Instance Status, Disk Space, and RAM Usage on both instances, with SNS email notifications.

**(Placeholder: Insert Architecture Diagram Image Here)**
*Link to exported Miro diagram or embed the image directly.*

---

# Technology Stack

* **Game Engine:** Unreal Engine 5 (using C++)
* **Version Control:** Perforce Helix Core (P4D - Self-hosted on EC2) | Git (Public mirror via `git-p4`)
* **Cloud Platform:** AWS (EC2, VPC, S3, Route 53 [planned], AWS Backup, CloudWatch, SNS, IAM)
* **Database:** PostgreSQL 14.x
* **VPN:** WireGuard (Self-hosted on EC2)
* **Operating System:** Ubuntu 22.04.5 LTS (Servers) | Windows (Client)
* **CI/CD (Planned):** Jenkins
* **QA/PM Tools (Planned):** JIRA, TestRail (or similar), Confluence, Miro
* **Infrastructure as Code (Planned):** Terraform
* **Other Tools:** `iptables`, `netfilter-persistent`, `fail2ban`, `ssh`, `scp`, `git-filter-repo`, P4V, DBeaver/psql

---

# QA Workflow & Processes (Current & Planned)

* **Version Control:**
    * *Primary:* Perforce (`//corp_rats-depot/`) hosts all project files, including large UE assets (`/Content`).
    * *Streams:* Basic stream setup in use (e.g., `//corp_rats-depot/main`). Developers work in dedicated development streams (to be fully defined). Workflow involves merging down from `main` and copying up completed work.
    * *Git Mirror:* This repository mirrors text-based files (code, scripts, configs, docs) from Perforce for public visibility using `git-p4`. `/Content` and `/Logs` are excluded.
* **Bug Tracking (Planned):** JIRA will be used for reporting, tracking, prioritizing, and managing defects found during testing.
* **Test Case Management (Planned):** TestRail (or a Confluence/Spreadsheet alternative) will be used to design, organize, and track the execution status of manual and automated test cases.
* **Testing Strategy (Planned):** Employing multiple layers of testing:
    * Unit Tests (C++/UE5 Automation Framework).
    * Integration Tests (e.g., game logic interacting with PostgreSQL).
    * API Tests (for planned backend API).
    * Performance Tests (Unreal Insights, PIX).
    * Manual Exploratory & Regression Testing.
    * Security & Network Considerations.
* **CI/CD (Planned):** Jenkins pipeline will automate build compilation, unit test execution, and reporting.

---

# Repository Contents

This Git repository serves as a public mirror of the primary Perforce depot, excluding large binary assets and transient logs. You will find:

* `/Source/`: C++ source code for the Unreal Engine game project.
* `/Config/`: Unreal Engine configuration files (`.ini`).
* `/Tests/`: Source code for automated tests (Unit, Integration, API).
* `/Database/`: SQL scripts for database schema/setup (if applicable).
* `/CI_CD/`: Jenkinsfile and related helper scripts for the CI/CD pipeline (when implemented).
* `/API/`: Swagger/OpenAPI specification for the backend API (when implemented).
* `/Documentation/`: Supporting documentation, potentially exported Confluence pages or design documents. May also include IaC code under a subfolder like `/iac/`.

The `/Content/` directory containing Unreal Engine assets (.uasset, .umap) and large `/Logs/` directories reside *only* in the Perforce depot.

---

# How to Explore

* **Infrastructure as Code:** (Planned) Terraform code defining the AWS infrastructure will be located in `/iac/`.
* **Unit Tests:** C++ test code can be found in `/Tests/UnitTests/` (or similar path within `/Source/`).
* **CI/CD Pipeline:** The pipeline definition can be found in the `Jenkinsfile` in the repository root (when implemented).
* **This README:** Provides the high-level overview.

---

# Skills Demonstrated

This project showcases proficiency and practical experience in:

* **QA Process & Management:** Test Planning, Test Case Design (TestRail/Alternative), Bug Tracking & Triage (JIRA), QA Metrics definition (Planned).
* **Test Execution:** Manual Functional Testing, Exploratory Testing.
* **Test Automation:** Unit Testing (C++/UE5), Integration Testing concepts, API Testing (Postman/Newman - Planned).
* **CI/CD for QA:** Understanding build pipelines, integrating automated tests, analyzing results (Jenkins - Planned).
* **Version Control Systems:**
    * *Perforce:* Server setup (P4D on EC2), stream setup/workflow, user/permission management, typical client usage (P4V/CLI).
    * *Git:* Mirroring strategies (`git-p4`), history filtering (`git-filter-repo`), standard Git workflow for the mirror repo.
* **Cloud Infrastructure (AWS):** VPC Design (Public/Private Subnets), EC2 instance management, Security Groups, Network Routing, NAT concepts, Elastic IPs, AWS Backup configuration.
* **Linux Server Administration:** Ubuntu setup, service management (`systemd`), firewall configuration (`iptables`, `netfilter-persistent`), basic hardening (`fail2ban`), troubleshooting.
* **Networking:** VPN setup & configuration (WireGuard, split tunnel), TCP/IP, UDP, DNS basics, SSH, SCP.
* **Databases:** Basic PostgreSQL setup and interaction.
* **Monitoring & Alerting:** CloudWatch Metrics & Alarms setup (EC2 standard + Custom via Agent), SNS notifications.
* **(Planned) Infrastructure as Code:** Terraform for AWS resource management.

---

# Next Steps / Future Plans

The immediate next steps focus on establishing the core QA processes and starting test automation within a CI/CD framework:

* Finalize JIRA & TestRail (or alternative) setup.
* Implement initial Unit Tests for core game logic.
* Set up Jenkins server and basic UE5 build pipeline.
* Integrate Unit Test execution into the CI pipeline.
* Begin implementation of Infrastructure as Code (Terraform).
* (Ongoing) Develop game features and expand test coverage (Manual & Automated).
* (Future) Implement backend API, API tests, performance tests, enhance monitoring, create demo video.

---

**(Placeholder: Link to Demo Video Here)**

---

**(Placeholder: Link to Detailed Public Documentation Here - e.g., Blog Post, GitHub Pages, Exported PDFs)**

---