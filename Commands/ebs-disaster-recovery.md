# Disaster Recovery using EBS Snapshot 

Simulate disaster recovery by restoring an EBS snapshot from North Virginia Region into Mumbai Region

Tasks performed:

- Restored EBS from copied snapshot
- Attached volume to Mumbai Region App-1
- Mounted filesystem
- Verified recovered data
- Expanded volume from 1GB → 2GB
- Extended filesystem without reboot

---

### Step 1 — Create Volume from Snapshot

AWS Console:

EC2 → Snapshots → Actions → Create Volume

Configuration:

| Setting | Value |
|---|---|
| Region | Mumbai |
| Type | gp3 |
| Size | 1 GiB |

---

### Step 2 — Attach Volume

Attached restored volume to:

- Mumbai Region App-1

Device name used:

```bash
/dev/sdf
```

---

#C# Step 3 — Verify Attached Volume

```bash
lsblk
```

Output:

```bash
NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
nvme0n1       259:0    0   8G  0 disk
├─nvme0n1p1   259:1    0   8G  0 part /
nvme2n1       259:5    0   1G  0 disk
```

---

### Step 4 — Mount Restored Volume

Created mount point:

```bash
sudo mkdir /data
```

Mounted volume:

```bash
sudo mount /dev/nvme2n1 /data
```

---

### Step 5 — Initial Issue Encountered

Initially:

```bash
cat /data/sample.txt
```

returned:

```bash
No such file or directory
```

Root cause:

- Snapshot was created before sample file existed.

This helped understand an important disaster recovery lesson:

> Infrastructure backup success does not guarantee application data backup success.

---

### Step 6 — Recreated Snapshot with Proper Data

Back in Region-1:

```bash
echo "Hello from Region-1 DR Test" | sudo tee /data/sample.txt
```

Created new snapshot and copied it again to Mumbai.

---

### Step 7 — Filesystem Repair

Encountered mount error:

```bash
wrong fs type, bad superblock
```

Verified filesystem:

```bash
sudo file -s /dev/nvme2n1
```

Output:

```bash
SGI XFS filesystem data
```

Fixed using:

```bash
sudo xfs_repair -L /dev/nvme2n1
```

Mounted successfully after repair.

---

### Step 8 — Verify Disaster Recovery Data

```bash
cat /data/sample.txt
```

Output:

```bash
Hello from Region-1 DR Test
```

Cross-region recovery successful.

---

### Step 9 — Expand EBS Volume

Modified volume in AWS Console:

```text
1GB → 2GB
```

Verified:

```bash
lsblk
```

---

### Step 10 — Extend Filesystem Without Reboot

Used:

```bash
sudo xfs_growfs /data
```

Verified final size:

```bash
df -h
```

---

### Final Outcome

Successfully demonstrated:

- EBS snapshot recovery
- Cross-region disaster recovery
- Filesystem repair
- Live volume expansion
- Zero downtime storage scaling
