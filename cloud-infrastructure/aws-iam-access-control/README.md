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
Project completion was validated through multiple authorization tests performed using separate IAM user accounts.

### IAM Group Membership Validation

Verified successful assignment of users to their respective IAM groups:

- EC2-Admin
- EC2-Support
- S3-Support

Group membership ensured permissions were inherited through role assignments rather than direct user permissions.

### Access Boundary Validation

Authenticated as each IAM user and verified authorization behavior against assigned permissions.

#### user-1 (S3-Support)

Successfully:

- Accessed Amazon S3 resources
- Viewed available buckets

Denied:

- Amazon EC2 access

Result:

The account operated strictly within its assigned permission scope.

#### user-2 (EC2-Support)

Successfully:

- Accessed Amazon EC2
- Viewed existing instances

Denied:

- EC2 instance state changes
- Amazon S3 access

Result:

The account maintained read-only visibility while preventing administrative actions.

#### user-3 (EC2-Admin)

Successfully:

- Accessed EC2 resources
- Executed instance stop operations
- Changed instance state

Result:

Administrative permissions functioned as expected without authorization errors.

Screenshots were captured throughout the testing process, including user assignments, authorization denials, and successful administrative actions. 【1-ca6691】【2-d80243】

---

## Security Concepts Demonstrated

### Identity and Access Management (IAM)

Implemented user access controls through centralized identity administration using AWS IAM users, groups, and policies. 【1-ca6691】

### Role-Based Access Control (RBAC)

Permissions were assigned to groups representing organizational job functions, allowing users to inherit appropriate privileges based on their responsibilities. 【1-ca6691】

### Principle of Least Privilege (PoLP)

Accounts received only the permissions required to perform their assigned duties. Access outside of those responsibilities was restricted through policy enforcement. 【1-ca6691】

### Authorization Controls

User activities were tested to ensure IAM policies permitted approved actions while preventing unauthorized operations. 【1-ca6691】

### Access Governance

Permission administration was centralized through group-based assignments, improving consistency and reducing the risk of individual permission sprawl. 【1-ca6691】

---

## Key Takeaways

- Implemented role-based access controls using AWS IAM groups and policies.
- Evaluated AWS managed policies and customer-defined inline policies.
- Validated authorization boundaries through direct user testing.
- Applied least-privilege principles to cloud resource access.
- Gained practical experience with cloud identity administration.
- Developed foundational IAM skills directly applicable to identity governance and access management environments.

---

## Artifacts

Suggested artifact structure:

```text
artifacts/
├── iam-user-review.png
├── iam-group-membership.png
├── s3-support-access-test.png
├── ec2-support-readonly-validation.png
├── access-denied-ec2-stop.png
├── ec2-admin-instance-stop-success.png
├── authorization-testing-summary.png
└── CMIT_326_Intro_AWS_IAM.pdf
```

---

## IAM and GRC Relevance

### IAM Analyst

This project demonstrates:

- User administration
- Group administration
- Access provisioning
- Permission inheritance
- Role assignment
- Authorization validation
- Access governance concepts

### GRC Analyst

This project demonstrates:

- Least privilege implementation
- Policy-based access control
- Security control validation
- Identity governance concepts
- Access review processes
- Compliance-oriented permission management

---

## Repository Structure

```text
cloud-infrastructure/
└── aws-iam-access-control/
    ├── README.md
    └── artifacts/
        ├── CMIT_326_Intro_AWS_IAM.pdf
        └── Intro_to_AWS_IAM.docx
```
