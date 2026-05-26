# Phase 1: Region-1 Infrastructure Setup 

## Project Overview

This phase focuses on building the core networking and compute infrastructure in AWS Region-1 (`N. Virginia / us-east-1`).

---

## Region Information

| Component  | Value                     |
| ---------- | ------------------------- |
| Region     | `us-east-1 (N. Virginia)` |
| VPC Name   | `vpc-1`                   |
| CIDR Block | `192.168.0.0/24`           |

---

## VPC Configuration

| Resource       | Configuration   |
| -------------- | --------------- |
| VPC            | `vpc-1`         |
| CIDR           | `192.168.0.0/24` |
| DNS Hostname   | Enabled         |
| DNS Resolution | Enabled         |

---

## Subnet Configuration

| Subnet | CIDR             | Type    | Availability Zone |
| ------ | ---------------- | ------- | ----------------- |
| SN1    | `192.68.0.0/28`  | Public  | us-east-1a        |
| SN2    | `192.68.0.16/28` | Public  | us-east-1b        |
| SN3    | `192.68.0.32/28` | Private | us-east-1a        |
| SN4    | `192.68.0.48/28` | Private | us-east-1b        |
| SN5    | `192.68.0.64/28` | Private | us-east-1a        |
| SN6    | `192.68.0.80/28` | Private | us-east-1b        |

---

## Internet Gateway

| Resource         | Name       |
| ---------------- | ---------- |
| Internet Gateway | `vpc1-igw` |

The Internet Gateway was attached to `vpc-1` to allow public internet access.

---

## NAT Gateway

| Resource    | Name       |
| ----------- | ---------- |
| NAT Gateway | `vpc1-nat` |

| Configuration | Value    |
| ------------- | -------- |
| Subnet        | SN2      |
| Elastic IP    | Attached |

The NAT Gateway allows private instances to access the internet securely without exposing them publicly.

---

## Route Table Configuration

### Public Route Table

| Destination     | Target           |
| --------------- | ---------------- |
| `192.68.0.0/24` | local            |
| `0.0.0.0/0`     | Internet Gateway |

Associated Subnets:

* SN1
* SN2

---

### Private Route Table

| Destination     | Target      |
| --------------- | ----------- |
| `192.68.0.0/24` | local       |
| `0.0.0.0/0`     | NAT Gateway |

Associated Subnets:

* SN3
* SN4
* SN5
* SN6

---

## EC2 Instance Configuration

| Instance    | AMI                   | Subnet | Type    |
| ----------- | --------------------- | ------ | ------- |
| Jump Server | Amazon Linux 2023     | SN1    | Public  |
| App-1       | Amazon Linux 2023     | SN3    | Private |
| App-2       | Ubuntu                | SN4    | Private |
| SQL Server  | Amazon Linux / Ubuntu | SN5    | Private |

---

## Security Group Configuration

### Jump Server Security Group

| Type | Port | Source |
| ---- | ---- | ------ |
| SSH  | 22   | My IP  |

---

### Private Server Security Group

| Type | Port | Source                     |
| ---- | ---- | -------------------------- |
| SSH  | 22   | Jump Server Security Group |

---

## Key Pair

| Resource | Name             |
| -------- | ---------------- |
| Key Pair | `devops-key.pem` |

The same key pair was used for all EC2 instances.

---

## NAT Gateway Validation

Tested internet connectivity from private servers.

## Amazon Linux

```bash
sudo yum update -y
```

---

## Ubuntu

```bash
sudo apt update -y
```

Successful package updates confirmed that private servers accessed the internet through the NAT Gateway.

---

---

## Screenshots Section 

---

## 1. VPC Dashboard

![VPC Dashboard](./Screenshots/Region-1%20Infrastructure%20Setup/vpc-dashboard.png)


---

## 2. Subnet Configuration

> Upload Screenshot Here

```text
screenshots/subnet-list.png
```

---

## 3. Route Tables

> Upload Screenshot Here

```text
screenshots/route-tables.png
```

---

## 4. Internet Gateway

> Upload Screenshot Here

```text
screenshots/internet-gateway.png
```

---

## 5. NAT Gateway

> Upload Screenshot Here

```text
screenshots/nat-gateway.png
```

---

## 6. EC2 Instance List

> Upload Screenshot Here

```text
screenshots/ec2-instances.png
```

---

## 7. Security Groups

> Upload Screenshot Here

```text
screenshots/security-groups.png
```

---

## 8. SSH Access from Jump Server to Private Server

> Upload Screenshot Here

```text
screenshots/ssh-private-server.png
```

---

## 9. NAT Internet Access Test

> Upload Screenshot Here

```text
screenshots/private-server-internet-access.png
```

---

