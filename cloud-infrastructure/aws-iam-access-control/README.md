# AWS IAM Access Control and Role-Based Authorization

## Overview

This project focused on implementing and validating access controls within Amazon Web Services (AWS) using AWS Identity and Access Management (IAM). User permissions were assigned through IAM groups and policies to support role-based access requirements across Amazon S3 and Amazon EC2 resources.

The implementation demonstrated how managed policies and inline policies can be used to enforce authorization boundaries, apply the Principle of Least Privilege (PoLP), and support scalable identity administration.

---

## Technologies Used

- Amazon Web Services (AWS)
- AWS Identity and Access Management (IAM)
- Amazon EC2
- Amazon S3
- AWS Managed Policies
- IAM Inline Policies
- AWS Management Console

---

## Skills Demonstrated

- Identity and Access Management (IAM)
- Role-Based Access Control (RBAC)
- Principle of Least Privilege (PoLP)
- Authorization Validation
- Access Governance
- Policy Review and Analysis
- User and Group Administration
- Cloud Security Fundamentals

---

## Environment

The project was performed within an AWS cloud environment containing preconfigured IAM users, IAM groups, Amazon EC2 resources, and Amazon S3 resources.

Three business roles were modeled using IAM groups:

| Role | Group | Access Level |
|--------|--------|--------|
| S3 Support | S3-Support | Read-only access to Amazon S3 |
| EC2 Support | EC2-Support | Read-only access to Amazon EC2 |
| EC2 Administrator | EC2-Admin | View, start, and stop EC2 instances |

Users were assigned to groups based on job responsibilities and inherited permissions through policy attachments. 【1-8b359b】

---

## Technical Implementation

### IAM Policy Review

Reviewed multiple IAM permission models including AWS managed policies and customer-defined inline policies.

Policies examined included:

- AmazonS3ReadOnlyAccess
- AmazonEC2ReadOnlyAccess
- EC2-Admin-Policy

The review focused on understanding how actions, resources, and permission scopes are defined within IAM policies. 【1-8b359b】

### Role-Based Access Control

User accounts were assigned to IAM groups that reflected organizational responsibilities.

Assignments included:

| User | Group |
|--------|--------|
| user-1 | S3-Support |
| user-2 | EC2-Support |
| user-3 | EC2-Admin |

This approach centralized permission management and reduced the need to assign permissions directly to individual users. 【1-8b359b】

### Authorization Testing

Access validation was performed by authenticating as multiple users and verifying their effective permissions.

#### S3 Support Validation

Verified:

- Successful access to Amazon S3 resources
- Denied access to Amazon EC2 resources

The user was able to browse S3 resources but was unable to perform EC2 actions due to insufficient permissions. 【1-8b359b】

#### EC2 Support Validation

Verified:

- Successful visibility into EC2 resources
- Inability to modify EC2 instances
- Denied access to S3 resources

The user was able to view EC2 instances but received authorization errors when attempting administrative actions. 【1-8b359b】

#### EC2 Administrator Validation

Verified:

- Successful access to EC2 resources
- Ability to stop EC2 instances
- Administrative control over instance state

The EC2 administrator successfully executed an instance stop action against the LabHost system, demonstrating elevated privileges granted through the assigned IAM policy. 【1-8b359b】【2-314b3a】

---

## Validation

Project completion was validated through:

### IAM Group Membership Verification

Confirmed user membership within:

- EC2-Admin
- EC2-Support
- S3-Support

### Authorization Boundary Testing

Validated that:

- Users could access authorized resources
- Users were restricted from unauthorized actions
- Policy enforcement occurred as expected

### Administrative Action Execution

Verified successful EC2 instance lifecycle management by an authorized administrative account.

Screenshots document user assignments, authorization testing, and successful EC2 administrative actions. 【2-314b3a】【1-8b359b】

---

## Security Concepts Demonstrated
