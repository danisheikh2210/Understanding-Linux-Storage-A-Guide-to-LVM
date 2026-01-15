# Linux LVM Hands-On Lab (with AWS EBS)

This repository contains a **step-by-step hands-on lab** to understand and implement
**Linux Logical Volume Manager (LVM)** using **AWS EBS volumes**.

This lab demonstrates how Linux storage works in layers and how LVM enables
**dynamic storage expansion without downtime**, which is critical in real-world
cloud and production environments.

---

## 🎯 Lab Objectives

By completing this lab, you will learn how to:

- Attach multiple EBS volumes to an EC2 instance
- Create Physical Volumes (PV)
- Create a Volume Group (VG)
- Create and manage Logical Volumes (LV)
- Format and mount storage
- Extend disk storage **online without downtime**

---

## 🧰 Prerequisites

- AWS EC2 Linux instance (Amazon Linux / Ubuntu / RHEL)
- 2 EBS volumes attached to the instance
- Root or sudo access
- Basic Linux command knowledge

---

## 🏗️ Linux Storage Architecture

EBS Volumes
↓
Physical Volumes (PV)
↓
Volume Group (VG)
↓
Logical Volume (LV)
↓
Filesystem (ext4)
↓
Mount Point (/appdata)

yaml
Copy code

---

## 🧪 Lab Steps

### 🔹 Step 1: Verify Attached Disks
```bash
lsblk
Expected disks:

bash
Copy code
/dev/nvme1n1
/dev/nvme2n1
🔹 Step 2: Create Physical Volumes (PV)
bash
Copy code
pvcreate /dev/nvme1n1
pvcreate /dev/nvme2n1
Verify:

bash
Copy code
pvs
🔹 Step 3: Create Volume Group (VG)
bash
Copy code
vgcreate vg_data /dev/nvme1n1 /dev/nvme2n1
Verify:

bash
Copy code
vgs
🔹 Step 4: Create Logical Volume (LV)
bash
Copy code
lvcreate -L 30G -n lv_app vg_data
Verify:

bash
Copy code
lvs
🔹 Step 5: Create Filesystem
bash
Copy code
mkfs.ext4 /dev/vg_data/lv_app
🔹 Step 6: Mount Logical Volume
bash
Copy code
mkdir /appdata
mount /dev/vg_data/lv_app /appdata
Verify:

bash
Copy code
df -h
🔹 Step 7: Extend Logical Volume (Online Resize)
bash
Copy code
lvextend -L +2G /dev/vg_data/lv_app
resize2fs /dev/vg_data/lv_app
Verify:

bash
Copy code
df -h
✔ No reboot
✔ No unmount
✔ No downtime

📌 Key Commands Summary
bash
Copy code
lsblk
pvcreate
vgcreate
lvcreate
mkfs.ext4
mount
lvextend
resize2fs
df -h
🧠 Real-World Use Cases
Expanding production disks without downtime

Managing cloud storage dynamically

Combining multiple EBS volumes into one storage pool

Safe disk expansion in DevOps environments

📖 Blog Reference
🔗 Understanding Linux Storage: A Guide to LVM
https://linux-volume-mount.hashnode.dev/understanding-linux-storage-a-guide-to-lvm

🚀 What This Lab Demonstrates
Strong Linux fundamentals

Real-world cloud storage management

Production-safe disk operations

Hands-on DevOps skillset

👤 Author
Danish Sheikh
DevOps | AWS | Linux
Hashnode: https://hashnode.com/@danisheikh2210
