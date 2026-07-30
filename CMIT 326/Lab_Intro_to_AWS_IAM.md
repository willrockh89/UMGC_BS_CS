# Lab: Introduction to AWS Identity and Access Management (IAM)

---
## Room Metadata
* **Platform:** AWS Academy / UMGC
* **Category:** Blue Team / Identity & Access Management / Cloud Security
* **Project:** Introduction to AWS IAM
* **Difficulty:** Easy
* **Status:** Completed
---

## Objective
**Goal:** Audit pre-configured AWS IAM identities and policies, implement Role-Based Access Control (RBAC) by mapping users to specialized support and administrator groups, and validate the Principle of Least Privelege by verifying service-level access restrictions across multiple user accounts.

---

## Tools Used

| Category | Utilities & Services |
| :--- | :--- |
| **Cloud Identity** | AWS Identity and Acess Management (IAM users, User Groups, Managed Policies, Inline Policies) |
| **Compute & Storage** | Amazon EC2 Console, Amazone S3 Console |
| **Administration** | Custom IAM Console Sign-in Endpoint, Incognito Browser Sessions |

---

## Methodology

### Phase 1: IAM Infrastructure & Policy Audit
1. Navigated to the AWS IAM Console and audited default user accounts (`user-1`, `user-2`, `user-3`).
2. Confirmed that individual user accounts possessed zero direct permissions or group memberships prior to provisioning.
3. Examined pre-created User Groups and their attached authorization structures:
   * **`EC2-Support`:** Attached to AWS Managed Policy `AmazonEC2ReadOnlyAccess`, providing read-only API actions across EC2, CloudWatch, and Auto Scaling.
   * **`S3-Support`:** Attached to AWS Managed Policy `AmazonS3ReadOnlyAccess`, granting `Get` and `List` permissions for Amazon S3 buckets and objects.
   * **`EC2-Admin`:** Configured with custom Inline Policy `EC2-Admin-Policy`, granting `ec2:Describe*`, `ec2:StartInstances`, and `ec2:StopInstances` permissions.

### Phase 2: Role-Based Access Control (RBAC) Implementation
1. Added `user-1` to the `S3-Support` group to grant S3 storage administration visibility.
2. Added `user-2` to the `EC2-Support` group to enable EC2 read-only monitoring.
3. Added `user-3` to the `EC2-Admin` group to provide EC2 lifecycle control capabilities.
4. Verified that all three groups reflected an active membership count of `1`.

### Phase 3: Identity Verification & Least Privilege Validation
1. `user-1` **(S3 Support Specialist):**
  * Logged in using account credentials via the account-specific IAM sign-in URL.
  * Verified full read access to list S3 buckets and inspect contents.
  * Navigated to Amazon EC2 instances; access was  blocked with `You are not authorized to perform this operation`.
2. `user-2` **(EC2 Support Specialist):**
  * Logged into the AWS Management Console as `user-2`.
  * Navigated to EC2 instances in region `us-east-1` and verified read visibility of `Bastion Host` (`i09cf76927ac6feb66`) and `LabHost` (`i-07b9291f565c4ec8b`).
  * Executed an instance `Stop` action on `LabHost`. The Request was denied due to missing write privileges (`You are not authorized to perform this operation`).
  * Navigated to Amazon S3 and verified the explicit access block (`You don't have permissions to list buckets`).
3. `user-3` **(EC2 Administrator):**
  * Logged into the AWS Management Console as `user-3`.
  * Navigated to EC2 instances and selected `LabHost` (`i-07b9291f565c4ec8b`).
  * Issued instance `Stop` command and successfully authorized and transitioned `LabHost` to `stopping`/`stopped` state.

## Key Findings
  * **AWS Account ID:** `<AWS_ACCOUNT_ID>` *(Sanitized for OPSEC)*
  * **Federated UserID:** `<FEDERATED_USER_ID>` *(Sanitized for OPSEC)*
  * **Group ARN (`EC2-Admin`):** `arn:aws:iam::<AWS_ACCOUNT_ID>:group/spl66/EC2-Admin`
  * **Target EC2 Instance:** `LabHost` (`i-07b9291f565c4ec8b`, `10.1.11.198` in `us-east-1a`)
  * **Policy Architecture Distinction:** Reaffirmed that AWS Managed Policies (`AmazonEC2ReadOnlyAccess`, `AmazonS3ReadOnlyAccess`) offer scalable, multi-entity assignment, whereas Inline Policies (`EC2-Admin-Policy`) enforce tightly scoped, single-entity granular permissions.
  * **Access Boundary Enforcement:** Confirmed default-deny security enforcement across all AWS API endpoints when corresponding permissions statements are absent.

---

## Reflection

### Real-World Context
In enterprise cloud environments—much like military aviation tool control and maintenance quality assurance where strict privilege boundaries prevent unauthorized component modifications—enforcing the Principle of Least Privilege via IAM user groups eliminates unnecessary administrative exposure. Mapping identities to job-function-specific groups ensures operators possess only the precise permissions required to complete their assigned scope of work. This operational model simplifies access auditing, mitigates insider threat risks, and prevents accidental disruption of production cloud assets.