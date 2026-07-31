# Lab: Working with Amazon EBS Volumes and Snapshots

---

## Room Metadata
* **Platform:** AWS Academy / UMGC
* **Category:** Cloud Storage / Data Integrity & System Administration
* **Project:** Lab: Working with EBS
* **Difficulty:** Medium
* **Status:** Completed

---

## Objective
**Goal:** Provision an Amazon Elastic Block Store (EBS) volume, attach and mount it to an active EC2 Linux instance, format an ext3 file system, test file system persistence, perform data recovery using EBS snapshots, and restore point-in-time data to a secondary mount point[cite: 3].

---

## Tools Used

| Category | Utilities & Services |
| :--- | :--- |
| **Cloud Storage** | AWS EBS (General Purpose SSD `gp3`), EBS Snapshots[cite: 3] |
| **Compute & Remote Access** | Amazon EC2 (Amazon Linux 2023), SSH (`PuTTY` / OpenSSH)[cite: 3] |
| **Linux Utilities** | `mkfs.ext3`, `mount`, `df`, `tee`, `/etc/fstab`, `rm`[cite: 3] |

---

## Methodology

### Phase 1: Provisioning & Attaching the EBS Volume
1. Provisioned a new 1 GiB `gp3` EBS volume (`vol-00c674d45f34d96e9`) in the `us-east-1a` Availability Zone[cite: 3].
2. Attached `My Volume` (`vol-00c674d45f34d96e9`) to the target EC2 Linux instance (`i-0aabf446cf0200304`) as device `/dev/sdf`[cite: 3].

### Phase 2: Remote Connection & File System Configuration
1. Established an SSH session to the Amazon Linux 2023 instance (`ec2-user@<PRIVATE_IP>`) using OpenSSH public key authentication[cite: 3].
2. Formatted the unformatted block storage device `/dev/sdf` with an `ext3` file system using `mkfs.ext3`[cite: 3].
3. Created directory `/mnt/data-store` and mounted `/dev/sdf` using `sudo mount /dev/sdf /mnt/data-store`[cite: 3].
4. Configured persistent mounting by appending `/dev/sdf /mnt/data-store ext3 defaults,noatime 1 2` to `/etc/fstab` using `tee`[cite: 3].
5. Verified disk allocation using `df -h` (confirming `/dev/xvdf` mounted at 975M capacity)[cite: 3].

### Phase 3: Data Creation & Point-in-Time Snapshot
1. Wrote initial data to the volume: `sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"`[cite: 3].
2. Verified contents of `file.txt` using `cat /mnt/data-store/file.txt`[cite: 3].
3. Created a point-in-time EBS Snapshot of `My Volume` via the AWS Management Console to preserve state[cite: 3].

### Phase 4: Simulating Data Loss & Recovery Validation
1. Simulated accidental data deletion by executing `sudo rm /mnt/data-store/file.txt`[cite: 3].
2. Verified file removal using `ls /mnt/data-store/` (returning only `lost+found`)[cite: 3].
3. Restored the EBS snapshot into a new 1 GiB volume (`Restored Volume` / `vol-065910e00977923ba`) and attached it as `/dev/sdg`[cite: 3].
4. Created secondary mount point `sudo mkdir /mnt/data-store2` and mounted `/dev/sdg` to `/mnt/data-store2`[cite: 3].
5. Verified complete data recovery by inspecting `/mnt/data-store2/`, confirming the restored presence and intact contents of `file.txt`[cite: 3].

---

## Key Findings

* **Volume ID Created:** `vol-00c674d45f34d96e9` (1 GiB, gp3, 3000 IOPS, 125 MB/s Throughput)[cite: 3].
* **Restored Volume ID:** `vol-065910e00977923ba` provisioned directly from snapshot backup[cite: 3].
* **Block Device Mapping:** Linux kernel mapped AWS `/dev/sdf` to local device path `/dev/xvdf`[cite: 3].
* **Recovery Status:** 100% data recovery verified from point-in-time snapshot following simulated local deletion[cite: 3].

---

## Reflection

### Real-World Context
In enterprise cloud engineering and high-availability operations, block storage management and point-in-time backup policies serve as the primary defense against catastrophic data loss, ransomware corruption, and human error. Mounting secondary EBS volumes and standardizing automated snapshot regimes ensure rapid Recovery Time Objectives (RTO) and minimal Recovery Point Objectives (RPO). Applying precise system administration—such as proper `/etc/fstab` formatting and non-destructive restoration to isolated mount points—embodies function over form: establishing deterministic, audit-ready data resilience across critical enterprise infrastructure.
