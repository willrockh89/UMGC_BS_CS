# Lab: Scale and Load Balance Your Architecture

---

## Room Metadata
* **Platform:** AWS Academy / UMGC
* **Category:** Cloud Architecture / Infrastructure & System Hardening
* **Project:** Lab 6: Scale and Load Balance Your Architecture
* **Difficulty:** Medium
* **Status:** Completed

---

## Objective
**Goal:** Deploy an Elastic Load Balancer (ELB) and an Auto Scaling group to distribute application traffic across multiple EC2 instances, achieve high availability across Availability Zones, and configure dynamic scaling based on CloudWatch CPU load metrics[cite: 1].

---

## Tools Used

| Category | Utilities & Services |
| :--- | :--- |
| **Compute & Images** | AWS EC2, Amazon Machine Image (AMI: `WebServerAMI`)[cite: 1] |
| **Load Balancing** | AWS Application Load Balancer (`LabELB`), Target Groups (`LabGroup`)[cite: 1] |
| **Scaling & Automation** | AWS Auto Scaling Group (`Lab Auto Scaling Group`), Launch Templates (`LabConfig`)[cite: 1] |
| **Monitoring & Alarms** | Amazon CloudWatch Alarms (High/Low CPU Utilization Tracking)[cite: 1] |
| **Networking & Security** | AWS VPC (`Lab VPC`), Public/Private Subnets, Web Security Group[cite: 1] |

---

## Methodology

### Phase 1: Custom Image Creation (AMI)
1. Verified operational status of base instance `Web Server 1` (`i-04ae3940298d6d062`)[cite: 1, 2].
2. Created a standardized image `WebServerAMI` (`ami-0e56f14dea72b1961`) to preserve boot disk configurations for rapid deployment[cite: 1, 2].

### Phase 2: Application Load Balancer (ALB) Setup
1. Configured target group `LabGroup` on `Lab VPC` using HTTP port 80[cite: 1].
2. Deployed layer-7 Application Load Balancer `LabELB` across `Public Subnet 1` and `Public Subnet 2` bound to `Web Security Group`[cite: 1].

### Phase 3: Launch Template & Auto Scaling Group Configuration
1. Built launch template `LabConfig` linking `WebServerAMI`, `t2.micro` instance type, `vockey` key pair, and detailed CloudWatch monitoring[cite: 1].
2. Configured `Lab Auto Scaling Group` spanning `Private Subnet 1` and `Private Subnet 2` with target capacity bounds (Min: 2, Desired: 2, Max: 6)[cite: 1].
3. Applied target tracking scaling policy `LabScalingPolicy` targeted at 60% average CPU utilization[cite: 1].

### Phase 4: Load Testing & Auto Scaling Verification
1. Confirmed health checks for newly spawned instances (`i-080ea6fcd8c14ea8d` and `i-086145f9314a8ce8e`) under `LabGroup`[cite: 1, 2].
2. Executed synthetic stress test via web application interface (`Load Test`) to drive CPU load[cite: 1, 2].
3. Monitored CloudWatch alarm `AlarmHigh`, validating automatic provisioning of additional EC2 capacity up to 4 active instances[cite: 1, 2].

### Phase 5: Decommissioning Base Hardware
1. Safely terminated original source instance `Web Server 1` (`i-04ae3940298d6d062`), verifying application continuity via `LabELB`[cite: 1, 2].

---

## Key Findings

* **AMI ID:** `ami-0e56f14dea72b1961` created successfully from base web server[cite: 2].
* **Load Balancer DNS:** `LabELB` successfully routing incoming HTTP requests across private subnet targets[cite: 1].
* **Auto Scaling Response:** Triggered scale-out event upon exceeding 60% average CPU load, automatically increasing active instances from 2 to 4 while maintaining fault tolerance[cite: 1, 2].
* **High Availability:** Instance distribution verified across multiple Availability Zones (`us-east-1a`, `us-east-1b`)[cite: 1, 2].

---

## Reflection

### Real-World Context
In enterprise cloud infrastructure—much like safety-critical flight propulsion systems—fault tolerance requires redundant design and real-time response to dynamic loads. Relying on a single compute node creates a critical single point of failure (SPOF). By leveraging automated scaling policies and load balancing across isolated Availability Zones, network availability transitions from manual intervention to predictable, programmatic resilience. This lab reinforces function over form: constructing self-healing architectures where infrastructure elasticity seamlessly adapts to operational demands without administrative downtime.
