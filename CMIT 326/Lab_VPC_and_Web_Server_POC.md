# Lab: AWS VPC Architecture & EC2 Web Server Provisioning

---

## Room Metadata
* **Platform:** AWS Academy / UMGC
* **Category:** Blue Team / Cloud Infrastructure & System Architecture
* **Room:** CMIT 326 - Proof-of-Concept VPC & Web Server
* **Difficulty:** Medium
* **Status:** Completed

---

## Objective
**Goal:** Design, build, and validate a custom Virtual Private Cloud (VPC) architecture on AWS, configure public and private subnets across multiple Availability Zones for High Availability, establish custom Security Group firewall rules, and deploy a bootstrap-configured Apache/PHP web server on Amazon EC2.

---

## Tools Used

| Category | Utilities & Services |
| :--- | :--- |
| **Cloud Network** | AWS VPC, Subnets, Internet Gateway (`lab-igw`), NAT Gateway (`lab-nat-public1-us-east-1a`), Route Tables |
| **Compute & Security** | Amazon EC2 (Amazon Linux 2023 / `t2.micro`), AWS Security Groups (`Web Security Group`) |
| **Services & Software** | Apache Web Server (`httpd`), PHP, MariaDB, `wget`, AWS S3 |

---

## Methodology

### Phase 1: Custom VPC Infrastructure Provisioning
1. Selected target region `us-east-1` (N. Virginia) and initiated VPC setup using the **VPC and more** workflow.
2. Auto-generated resources under tag prefix `lab` with IPv4 CIDR block `10.0.0.0/16`.
3. Configured initial single AZ subnets:
   * **Public Subnet:** `lab-subnet-public1-us-east-1a` (`10.0.0.0/24`)
   * **Private Subnet:** `lab-subnet-private1-us-east-1a` (`10.0.1.0/24`)
4. Attached Internet Gateway (`lab-igw`), provisioned single-AZ NAT Gateway (`lab-nat-public1-us-east-1a`), enabled DNS hostnames/resolution, and created `lab-vpc` (`vpc-0563eebaf2966e862`).

### Phase 2: High Availability Subnet & Security Configuration
1. Expanded subnet topology into a second Availability Zone (`us-east-1b`) by creating `lab-subnet-public2` (`10.0.2.0/24`) to support high-availability deployments.
2. Configured public route table associations (`lab-rtb-public`) to route external traffic through `lab-igw`.
3. Provisioned Security Group `Web Security Group` (`sg-0c9cfbc36ade932fd`) in `lab-vpc` with inbound HTTP (TCP 80) access enabled.

### Phase 3: EC2 Instance Provisioning & Automated Web Application Bootstrap
1. Launched an Amazon Linux 2023 EC2 instance (`t2.micro`) named `Web Server 1` (`i-0482e6923b86bf04d`) attached to SSH key pair `vockey`.
2. Placed the instance into `lab-vpc` -> `lab-subnet-public2` and associated `Web Security Group`.
3. Injected User Data script for automated bootstrap provisioning at instance boot:

```bash
#!/bin/bash
# Install Apache Web Server, PHP, and MariaDB
dnf install -y httpd wget php mariadb105-server

# Download and extract lab application files from S3
wget [https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip](https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip)
unzip lab-app.zip -d /var/www/html/

# Enable and start Apache HTTP service
chkconfig httpd on
service httpd start
```

---

### Phase 4: Verification & Web Application Tessting
1. Verified instaances status (2/2 checks passed) and public IP assignment (3.89.125.165).
2. Navigated to public IP in browser to confirm web application startup, displaying metadata for instance 'i-0482e6923b86bf04d' located in 'us-east-1b' with active PHP processing.

---

### Key Findings
* **VPC ID:** 'vpc-0563eebaf2966e862' ('10.0.0.0/16')

* **Security Group ID:** 'sg-0c9cfbc36ade932fd' ('Web Security Group')

* **EC2 Instance ID:** 'i-0482e6923b86bf04d' ('Web Server 1', '3.89.125.165' / Private '10.0.2.199')

* **Federated User ID:** 'voclabs/user2914574=willrock_herrera'

* **Deployment Validation:** Apache and PHP app deployed and served successfully via automated User Data boot script.

---

### Reflection

#### Real-World Context
Migrating on-premises data center workloads to AWS Infrastructure as a Service (IaaS) eliminates physical building lease fees, utility overhead, hardware lifecycle management, and emergency hardware maintenance. Designing custom Virtual Private Clouds with public/private subnet segmentation across multi-AZ configuration provides operational scalability and high availability. Automating web server configuration via User Data boot scripts ensures deterministic, repeatable infrastructure deployment (Infrastructure as Code philosophy), minimizing manual provisioning errors and establishing robust security perimeters.

