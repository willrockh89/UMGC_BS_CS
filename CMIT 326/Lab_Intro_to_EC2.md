# Lab: Introduction to Amazon EC2 Deployment & Lifecycle Management

## Metadata
* **Platform:** AWS / O'Reilly (UMGC CMIT 326)
* **Category:** Blue Team / Cloud Infrastructure Operations
* **Difficulty:** Easy
* **Status:** Completed

---

## Objective
The objective of this lab was to provision, monitor, configure, and manage the lifecycle of Amazon EC2 compute resources within an AWS VPC environment. The tasks encompassed deploying instances, modifying network access security groups, vertically scaling compute types and EBS storage, and auditing service quotas. Additionally, lifecycle protection mechanics were evaluated by testing termination behavior on live workloads. Successful execution resulted in a fully configured web server instance, dynamic network security adjustment, compute scaling, and verified instance termination.

---

## Tools Used
| Tool | Purpose | Primary Options Used |
| --- | --- | --- |
| **AWS Management Console** | Primary interface for resource provision and lifecycle control | EC2 Dashboard, Service Quotas, VPC Manager |
| **AWS EC2 Security Groups** | Stateful host-level virtual firewall configuration | Inbound Rules modification (`sg-01465da46bffe4a04`) |
| **Amazon Linux 2023** | Guest OS kernel and boot diagnostic monitoring | Kernel `6.1.61-85.141.amzn2023.x86_64` systemd logs |
| **AWS Service Quotas** | System-wide cloud resource limit inspection | Applied Quota Filter (`running on-demand`) |

---

## Methodology
### Phase 1: Instance Deployment & Network Identification
1. Provisions were validated for two running instances (`Bastion Host` - `i-0148141248917e1cd` and `Web Server` - `i-0f6e1c31b31000ece`) across separate Availability Zones (`us-east-1a` and `us-east-1b`).
2. Identified key network artifacts for the primary `Web Server` target: Public IP `18.212.121.150`, Private IP `10.0.2.178`, and Public DNS `ec2-18-212-121-150.compute-1.amazonaws.com`.

### Phase 2: System Boot Diagnostics & EC2 Monitoring
1. Connected to instance system log console to inspect early runtime and systemd startup behavior.
2. Evaluated kernel parameters and identified minor service startup warnings regarding missing native systemd unit files for legacy sysv services (`cfn-hup`).

### Phase 3: Network Security Group Reconfiguration
1. Located active security groups associated with the target VPC (`vpc-0a021710c43d54425`).
2. Modified stateful ingress rules on `Web Server security group` (`sg-01465da46bffe4a04`) to permit web traffic access to the instance.

### Phase 4: Elastic Compute & Storage Resizing
1. Stopped instance `i-0f6e1c31b31000ece` to perform offline instance type modification from `t2.micro` to `t2.small`.
2. Inspected root storage volume (`vol-031ce9c382dc44ae0`, `/dev/xvda`) sized at 8 GiB EBS storage.
3. Restarted instance and verified transition state from `Pending` back to `Running`.

### Phase 5: Resource Limit Audit & Lifecycle Termination
1. Inspected regional quota constraints in `us-east-1` via AWS Service Quotas.
2. Audited `Running On-Demand Standard (A, C, D, H, I, M, R, T, Z) instances` limit set to 5 instances.
3. Disabled instance termination protection (or authorized termination sequence) and initiated shutdown/termination for `i-0f6e1c31b31000ece`.
4. Confirmed state transition from `Shutting-down` to `Terminated` while public IP reallocated dynamically to `54.210.127.149`.

---

## Key Findings
* **Primary EC2 Instance ID:** `i-0f6e1c31b31000ece` (`Web Server`)
* **Secondary EC2 Instance ID:** `i-0148141248917e1cd` (`Bastion Host`, `t2.micro`)
* **Primary Security Group:** `sg-01465da46bffe4a04` (`Web Server security group`)
* **VPC ID:** `vpc-0a021710c43d54425`
* **EBS Volume ID:** `vol-031ce9c382dc44ae0` (8 GiB attached to `/dev/xvda`)
* **Initial / Final IPs:** Initial Public IP `18.212.121.150` $\rightarrow$ Termination Phase Dynamic IP `54.210.127.149`; Private IP `10.0.2.178`.
* **On-Demand Standard Instance Limit:** 5

---

## Architectural / Real-World Reflection
In enterprise production cloud architectures, manual lifecycle tasks performed in this lab—such as inline Security Group modifications and manual instance type resizing—are typically abstracted into Infrastructure as Code (IaC) pipelines (e.g., Terraform, AWS CloudFormation). Ad-hoc modifications via the management console introduce configuration drift and impair automated deployment state models. Maintaining predictable compute limits via AWS Service Quotas ensures operational continuity, avoiding deployment failures during auto-scaling events.

From a defensive operations standpoint, instances exposed directly to public subnets without Elastic IP bindings experience IP address churn upon state transitions, breaking standard firewall allowlists and telemetry mapping. Implementing Infrastructure-as-Code state enforcement alongside strict IAM role-based access control (RBAC) prevents unauthorized termination of mission-critical workloads while enforcing deterministic security group rules across all cloud boundaries.
