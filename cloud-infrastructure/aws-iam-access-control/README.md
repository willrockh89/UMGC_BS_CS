# AWS IAM Access Control & RBAC Implementation

## Project Overview

This project focused on designing and implementing AWS Identity and Access Management (IAM) controls using custom user groups, managed policies, and inline policies. Access was structured around organizational job functions and validated through multi-user authorization testing to enforce the Principle of Least Privilege (PoLP) and prevent unauthorized privilege escalation.

---

## Technologies Used

* **Cloud Provider:** Amazon Web Services (AWS)
* **Core Services:** AWS IAM, Amazon EC2, Amazon S3
* **Management Tools:** AWS Management Console, IAM Policy Simulator
* **Security Controls:** Role-Based Access Control (RBAC), JSON Policy Definition, Explicit Deny Enforcement

---

## Skills & Concepts Demonstrated

* **Identity & Access Management (IAM):** User, group, and policy lifecycle administration.
* **Role-Based Access Control (RBAC):** Group-based entitlement mapping to prevent permission drift.
* **Principle of Least Privilege (PoLP):** Restricting service access strictly to required job duties.
* **Authorization Troubleshooting:** Diagnosing implicit vs. explicit denials across EC2 and S3 resources.
* **Security Policy Engineering:** Evaluating AWS Managed Policies vs. Customer Inline Policies.

---

## Architecture & Access Control Model

Permissions were assigned strictly via **IAM User Groups** rather than direct user attachment, ensuring scalable governance across the organization.

| IAM User | Assigned Group | Attached Policy | Policy Type | Effective Authorization |
| :--- | :--- | :--- | :--- | :--- |
| `user-1` | **S3-Support** | `AmazonS3ReadOnlyAccess` | AWS Managed | Read-only access to S3 buckets and objects. |
| `user-2` | **EC2-Support** | `AmazonEC2ReadOnlyAccess` | AWS Managed | Read-only visibility into EC2, CloudWatch, and Auto Scaling. |
| `user-3` | **EC2-Admin** | `EC2-Admin-Policy` | Customer Inline | Read, Start, and Stop access for EC2 compute instances. |

---

## Execution & Authorization Validation

### 1. Environment & Group Setup
* Audited existing IAM principals to ensure zero unassigned or direct permission attachments.
* Provisioned functional groups (`S3-Support`, `EC2-Support`, `EC2-Admin`) and mapped specific managed and inline JSON policies.
* Assigned identities to their respective functional groups to establish baseline RBAC boundaries.

### 2. Authorization Boundary Testing
To verify least-privilege boundaries, active session testing was conducted across isolated user contexts:

* **S3 Specialist Validation (`user-1`):** Verified successful read access to S3 buckets. Attempted EC2 console navigation; received explicit authorization failure (`You are not authorized to perform this operation`).
* **EC2 Support Validation (`user-2`):** Confirmed full read/describe visibility across active EC2 instances (`Bastion Host`, `LabHost`). Executed an instance `Stop` command; action was blocked by an authorization denial. Confirmed zero access to S3 storage resources.
* **EC2 Administrator Validation (`user-3`):** Authenticated as `user-3` and issued a lifecycle state change against `LabHost` (`i-07b9291f565c4ec8b`). The `Stop Instance` API call executed successfully, transitioning the instance state to `Stopping`.

---

## Security Outcomes

* **Zero Direct Policy Attachment:** Standardized identity governance strictly through group inheritance, simplifying future access audits.
* **Implicit Deny Enforcement:** Verified that unassigned service domains (e.g., `user-1` attempting EC2 calls or `user-2` attempting S3 calls) defaulted to secure denial states.
* **Managed vs. Inline Boundary Control:** Utilized AWS Managed Policies for standardized read-only tiers while leveraging Customer Inline Policies to bound administrative compute privileges.

---

## Repository Structure

```text
cloud-infrastructure/
└── aws-iam-access-control/
    ├── README.md
    ├── assets/
    │   └── ec2-stop-instance-success.png
    └── documentation/
        ├── Technical_Overview.pdf
        └── Policy_Definitions.json
