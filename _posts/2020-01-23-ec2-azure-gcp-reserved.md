---
layout: post
title: EC2 Azure GCP reserved VM price comparison
ghissueid: 8
---

I was looking for a small virtual machine (or VPS virtual private server) for development.  Normally I would go with either digitalocean or linode.  Their prices are the same: $10 per month for 2GB vm and $20 for 4GB.  Because I know I will be using this VM for more than a year, I started look into the VM's that offered by big 3 cloud providers with long term discount.

Below are the findings.

## Amazon EC2

|type       |cpu/ram |term                       |price            |
|-----------|--------|---------------------------|-----------------|
|t3a.micro  |2cpu 1GB|standard 3-year all-upfront|$93 ($2.58/Mon)  |
|t3a.small  |2cpu 2GB|standard 3-year all-upfront|$186 ($5.16/Mon) |
|t3a.medium |2cpu 4GB|standard 3-year all-upfront|$372 ($10.33/Mon)|

Note price is for us-east-1 region.  Different regions has different price, for example, for northern-California region (us-west-1) t3a.medium costs $509 for 3-year all-upfront, for Tokyo region (ap-northeast-1), t3a.medium costs $553 for 3-year all-upfront. us-east-2, us-west-2 have the same price as us-east-1.

Also note t3a use AMD cpu's, t2 and t3 use Intel cpu's and are more expensive.  But in real world application it shouldn't make much a difference.


## Azure VM

|type |cpu/ram |term  |price      |
|-----|--------|------|-----------|
|B1S  |1cpu 1GB|3-year| $3.50/Mon |
|B1MS |1cpu 2GB|3-year| $7.69/Mon |
|B2S  |2cpu 4GB|3-year| $13.77/Mon|


## GCP Compute Engine VM Committed use 

|type |cpu/ram |term   |price     |
|-----|--------|-------|----------|
|N1   |1cpu 2GB|3-year |$15.82/Mon|
|N1   |1cpu 4GB|3-year |$19.16/Mon|


Looks like AWS has best price if you just want a 2-4GB VM without considering other factors.

