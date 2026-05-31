# Route 53
Objectives: 
* Connect my domain zentechs.site
* To both Load Balancers using Route 53.

## Creating Hosted Zone in Route53
* In, amazone route 53. created a hosted zone named "zentechs.site"
* Type: Public Hosted Zone
* After creation, route 53 generated 4 Name Servers (NS records)
---
[hosted name screenshot ]

## Updating Domain Nameservers
* I bought zentechs.site from Godaddy website.
* Replaced existing nameservers in Godaddy with the AWS Route 53 nameservers.
* saved it and verified it with DNS checker
* in DNS checker, typed zentechs.site on search bar and choosed NS record.
----
[DNS screenshoot ]
* also, verified Route 53 is active in terminal with command
```
nslookup -type=ns zentechs.site
```
----
[screenshot of terminal]

