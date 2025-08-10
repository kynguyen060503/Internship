---
title: "Create VPC and Networking"
date: 2025-08-09T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 1.1 </b> "
---

#### Access VPC Console
1. Sign in to **AWS Console**.

![VPC Console](/images/1-env-setup/aws-console.png)

2. Search for **VPC** in the search bar.

![Find VPC](/images/1-env-setup/find-vpc.png)

3. Click **VPC** service.

#### Create a New VPC
1. Click **Create VPC** → select **VPC and more**.

![Create VPC](/images/1-env-setup/create-vpc.png)

2. Configure:
   - **Name tag:** `migration-vpc`
   - **IPv4 CIDR:** `10.0.0.0/16`
   - **Number of AZs:** `2`
   - **Public subnets:** `2`
   - **Private subnets:** `2`
   - **NAT gateways:** `In 1 AZ`
   - **VPC endpoints:** `None`
   
![Create VPC1](/images/1-env-setup/create-vpc1.png)

![Create VPC2](/images/1-env-setup/create-vpc2.png)


3. Click **Create VPC** and wait ~5 minutes.

![Create VPC3](/images/1-env-setup/create-vpc3.png)

#### Notes
- **VPC ID:** `vpc-xxxxxxxxx`
- **Public Subnet 1:** `subnet-xxxxxxxxx`
- **Public Subnet 2:** `subnet-xxxxxxxxx`
- **Private Subnet 1:** `subnet-xxxxxxxxx`
- **Private Subnet 2:** `subnet-xxxxxxxxx`
