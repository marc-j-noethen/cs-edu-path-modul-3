# 🖥️ Bigger is Better – Attaching an additional 32 GB data disk to a VM

**Course:** Cloud & Enterprise Security – Virtual Machines & Compute | **Date:** 14 April 2026

---

## Task

**Objective:** Check an existing Ubuntu VM via SSH, attach a new 32 GB data disk using the Azure CLI, and then verify the additional storage with `lsblk`.

**Requirements:**
- Use the resource group and Ubuntu VM from the previous exercise.
- Run `lsblk` before and after attaching a new 32 GB data disk.
- Delete the resource group once the test is complete.

---

## Solution

### SSH check and attaching the data disk

```bash
# Before: via SSH on the VM
lsblk

# Locally or in Cloud Shell
az vm disk attach \
  --resource-group IntroAzureRG \
  --vm-name my-vm \
  --name myDataDisk \
  --new \
  --size-gb 32 \
  --sku Standard_LRS

# After: via SSH on the VM again
lsblk

# Clean-up
az group delete --name IntroAzureRG --yes
```

### Output

```text
Before:
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda       8:0    0   30G  0 disk
|-sda1    8:1    0 29.9G  0 part /
sdb       8:16   0    4G  0 disk

After:
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda       8:0    0   30G  0 disk
|-sda1    8:1    0 29.9G  0 part /
sdb       8:16   0    4G  0 disk
sdc       8:32   0   32G  0 disk
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|-------- --------|-------|---------------------|---|
| First `lsblk` | Record the initial state of the existing disks | Only the existing system and temporary disks are visible | ✅ |
| `az vm disk attach ... --new --size-gb 32 ...` | Creates and attaches a new managed data disk | The 32 GB disk is successfully attached to the VM | ✅ |
| Second `lsblk` | Visual check within the guest operating system | An additional block device entry, e.g. `sdc` with `32G`, appears | ✅ |
| `az group delete --name IntroAzureRG --yes` | Completely removes the lab environment | Resource group and VM are deleted | ✅ |

---

## Notes

- **Data disk:** An additional data disk extends a VM’s persistent storage independently of the operating system disk.
- **lsblk:** This command displays block devices and their sizes in the Linux system and is well suited for before-and-after comparisons.
- **Warning ⚠️:** Attaching the disk makes it visible to the operating system, but it is not yet partitioned, formatted or mounted.

