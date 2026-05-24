# Mounting EBS Volume in App-1

After attaching the EBS volume from AWS Console, I connected to the App-1 EC2 instance and verified whether the OS detected the new disk.

## Check Attached Volumes

```bash id="s0sfrq"
lsblk
```

Output:

```text id="6w26xw"
NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
nvme0n1       259:0    0   8G  0 disk
├─nvme0n1p1   259:1    0   8G  0 part /
├─nvme0n1p127 259:2    0   1M  0 part
└─nvme0n1p128 259:3    0  10M  0 part /boot/efi
nvme1n1       259:4    0   1G  0 disk
```

Here:

* `nvme0n1` → Root volume
* `nvme1n1` → Newly attached EBS volume

---

## Formatting the EBS Volume

The attached volume was a raw disk without any filesystem.
So I formatted it using the XFS filesystem.

```bash id="3n0cc8"
sudo mkfs -t xfs /dev/sdf
```

Explanation:

* `mkfs` → Make filesystem
* `-t xfs` → Use XFS filesystem type
* `/dev/sdf` → Attached EBS device

---

## Creating Mount Directory

Created a directory to mount the EBS volume.

```bash id="57jz7j"
sudo mkdir /data
```

---

## Mounting the Volume

Mounted the EBS volume to `/data`.

```bash id="0qx5fs"
sudo mount /dev/sdf /data
```

Verified using:

```bash id="cbqjbf"
df -h
```

Output:

```text id="s6g33j"
Filesystem        Size  Used Avail Use% Mounted on
devtmpfs          4.0M     0  4.0M   0% /dev
tmpfs             459M     0  459M   0% /dev/shm
tmpfs             184M  440K  183M   1% /run
/dev/nvme0n1p1    8.0G  1.6G  6.4G  20% /
tmpfs             459M     0  459M   0% /tmp
/dev/nvme0n1p128   10M  1.3M  8.7M  13% /boot/efi
tmpfs              92M     0   92M   0% /run/user/1000
```

---

## Creating Sample Data

Initially, I tried:

```bash id="q8rgtw"
echo "Hello from Region-1" > /data/sample.txt
```

But encountered this error:

```text id="vlh9qy"
-bash: /data/sample.txt: Permission denied
```

Reason:

* `/data` directory was owned by `root`
* Current user (`ec2-user`) did not have write permission

To solve this, I used:

```bash id="x4pqv2"
echo "Hello i'm arumugam from Region-1" | sudo tee /data/sample.txt
```

Verified using:

```bash id="w4g4oh"
cat /data/sample.txt
```

Output:

```text id="mw1t86"
Hello i'm arumugam from Region-1
```

---

# Key Learning

During this phase, I learned:

* How Linux detects attached block storage
* Difference between raw disk and filesystem
* How mounting works in Linux
* Basic Linux permission handling
* Why `sudo tee` works better than normal redirection in protected directories

This phase also simulates a real-world backup and disaster recovery workflow using EBS snapshots and cross-region replication.
