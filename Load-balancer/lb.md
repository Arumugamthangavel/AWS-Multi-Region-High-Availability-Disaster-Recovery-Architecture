# Load balancer 
Objectives:
* creating Load balancer on North Virginia region
* to v1 virtual private cloud
* with EFS index.html file

## Creating EFS in north virginia Region
* In EFS, Created File System named "v1-efs"
*  selected private subnet-1 from availability zone-A
*  selected private subnet-2 from availability zone-B  
---
[efs-screenshot ]

---
[subnet-efs-screenshot ]

## Re-Configure Security Group
* when creating vpc and ec2, i didn't NFS on security group of app-sg
* NFS is important for ec2 and efs connection
---
!security-group-nfs()

## EFS mounting
### App-1
* In App-1, installing efs utilies
```
sudo yum install amazon-efs-utils -y
```
* creating efs directory on app-1
```
mkdir /efs
```
* mounting efs on app-1
* inside /efs, created F1,F2,F3,F4
* inside F1, create file named "index.html"
* installing httpd

```
cd /efs
sudo mkdir F1 F2 F3 F4
echo "<h1>Welcome from F1</h1>" | sudo tee /efs/F1/index.html
sudo yum install httpd
```
* making sure httpd runnung on app-1
```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```
---
!efs-httpd-run()

* after that
* Mount F1-files into Web Directory
```
sudo mount --bind /efs/F1 /var/www/html
```
----
### App-2
* In App-2, installing efs utilies
```
sudo apt install amazon-efs-utils -y
```
* creating efs directory on app-2
```
mkdir /efs
```
* mounting efs on app-2
* inside F2, create file named "index.html" and
* installing apache2
```
cd /efs
sudo mkdir F1 F2 F3 F4
echo "<h1>Welcome from F2</h1>" | sudo tee /efs/F1/index.html
sudo apt install apache2
```

* also, making sure apache2 running on app-2
```
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```
----
!efs-apache2-run()

* after that
* Mount F2-files into Web Directory
```
sudo mount --bind /efs/F2 /var/www/html
```
---------------------

## Application Load Balancer
### Creating Target Group
* naming my target group "v1-target".
* selecting my v1 vpc and adding my app-1, app-2 ec2 instances.
----
!tg-lb()

### Creating Elastic Load Balancer
* creating Applicatin load balancer named "v1-lb".
* selected my v1 vpc.
* selected the subnet SN1-pub, SN2-pub. Because ELB must be internet accessible.
* for LB, http is enough in security group. i selected my app-sg.
* attached target group "v1-target".
---
!loadbalancer()

## Testing ELB
* Copied ELB DNS to check if it's working.
!f1()
!f2()



