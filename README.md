# AWS VIRTUAL PRIVATE CLOUD(VPC) AND NETWORK CONFIGURATION
This documentation outlines the process of creating and configuring an Amazon Web Services (AWS) Virtual Private Cloud (VPC). It covers subnet design, route table configuration, and the implementation of network security using Security Groups and Network Access Control Lists (NACLs). The guide also demonstrates launching an EC2 instance in a public subnet, highlighting best practices for network segmentation, controlled internet access, and layered security within an AWS environment.


## Table Of Contents
1. Introduction
2. Pre-requisites and Services Used
3. Planning the IP Address Space
4. Creating the VPC
5. Creating the Subnets
6. Creating and Associating the Route Tables to Subnets
7. Creating an Internet Gateway for the VPC
8. Configuring and Associating Security Groups
9. Configuring and Associating Network Access Control Lists
10. Creating and Launching an Elastic Cloud Compute (EC2) on the Public Instance
11. Create Flow Logs
12. Validation and Testing
13. Common Errors and Troubleshooting
14. Conclusion

## Introduction
An Amazon Web Services (AWS) Virtual Private Cloud (VPC) is a logically isolated network that enables secure communication between AWS resources, the internet, and on-premises networks. Subnetting refers to the logical division of an IP address range and allows the VPC to be segmented into smaller networks for improved organization, security, and traffic management. Security Groups and Metwork Access Control Lists are network firewalls that filter inbound and outbound traffic preventing unauthorized access. EC2 is a web service provided by AWS that allows you to launch and manage virtual servers, called instances, in the cloud. These instances provide scalable computing capacity, letting you run applications without the need to own physical hardware.

This document outlines the steps required to create an AWS VPC and configure subnets, along with related networking components as well as launching an EC2 instance in the Public Subnet.

## Pre-requisites and Services Used
* Access to AWS Console Dashboard
* An Active Subscription (Free/Paid Tier)
* Designated Availability Zone
* Basic Understanding of IP adressing and CIDR notation
* Virtual Private Cloud
* Subnets
* Route Table
* Security Groups
* Network Access Control Lists
* EC2 Instance
* Flow Logs

## Planning the IP Address Space
Planning the IP address space is a critical first step when designing an AWS Virtual Private Cloud (VPC). A well-structured IP plan helps prevent address conflicts, ensures efficient network segmentation, and supports future scalability. By selecting an appropriate private IP range and dividing it into clearly defined subnets, resources can be organized securely and managed effectively as the environment grows. Commonly used private IP address ranges include:

* 10.0.0.0/8
* 172.16.0.0/12
* 192.168.0.0/16

## Creating the VPC
* Sign in to AWS Console Dashboard as IAM user.
* Navigate to the VPC by searching for it using the Search bar.
* On the VPC overview page, click Create to start the process.
* Fill the necessary information for the VPC. This includes the name and Tag.
* Next is to configure the type of VPC. This could VPC Only where you have to configure your settings or VPC and more which is a pre-designed template.
* Choose the type of IP addressing you would use for network block.
* Check the cirles respectively for IPv4 CIDR manual input for IPv4 CIDR block and No IPv6 CIDR block for IPv4 CIDR block.
* Enusre your VPC encryption control is to None and Tenancy set to default. Other features require a paid subscription.
* Then click Create.

<p align="center"> <img width="735" height="385" alt="Screenshot 2026-02-02 225745" src="https://github.com/user-attachments/assets/2d9c7b2f-1782-4016-9201-eaea9bd59b5a" />


## Creating the Subnets
* Navigate to the menu section on the left side of the VPC dashboard and select subnets.
* On the Subnets page, click Create.
* Fill in your desired subnet name and select your preferred availabilty zone or choose None.
* Next step is to choose your IPv4 subnet CIDR block based off your IPv4 VPC CIDR block.
* Repeat the same process for subsequent subnets but note the IPv4 subnet CIDR block in each subnet to avoid repetitions and IP address conflict/overlapping.
* You can curate all subnets, add their details and set up the ip configurations together and create together.
* Finish the process by clicking Create.
<p align="center"> <img width="735" height="485" alt="Screenshot 2026-02-02 225704" src="https://github.com/user-attachments/assets/e17e2486-b9bb-464d-b68b-d8b490cd0b91" />

## Creating and Associating the Route Tables to Subnets
* On the VPC dashboard, select Route Table from the menu section.
* Add the name and select the VPC you want to use for the route table.
* Then click Create Route Table.
* On the Route table page, select your newly created RTB and navigate to the actions tab. This contains a host of functions you can apply to your RTB. Select the option to make it the main RTB for the VPC and save.
* The next step after making it the main RTB is to asssociate it to the public subnet. Select Edit Subnet Associations, check the box for public subnet and save association.
<p align="center"> <img width="735" height="385" alt="Screenshot 2026-02-02 235248" src="https://github.com/user-attachments/assets/58045c7c-476d-4c1c-a92d-eda06d38b756" />


## Creating an Internet Gateway for the VPC
An Internet Gateway lets your VPC connect to the internet. It allows resources in your VPC to send and receive traffic to and from the internet.
* Select the Internet gateway option from the menu section on the VPC dashboard.
* Choose a name and add an optional tag, then click Create Inetrnet Gateway.
* After creation, the IGW must be attached to the VPC to provide internet access. You can access this option by completing it when prompted or via the Actions tab.
* Select the VPC you intend to use and click Attach Internet Gateway.
* The next step is to edit routes in the Route Table to enable connnection to IGW.
* On the Route Table page, navigate to the Routes Section and click. Then select Edit routes.
* On this page, there is a pre-existing destination route rule. Go to Add route, then choose Internet gateway and select the relevant IGW. Then you save changes. 
* <p align="center"> <img width="735" height="418" alt="Screenshot 2026-02-03 001009" src="https://github.com/user-attachments/assets/47470ac0-87cb-49aa-95f3-f920a7d0053a" />

## Configuring and Associating Security Groups
* Select the Security Groups option from the menu section on the VPC dashboard
* Fill in the information in the Basic Details Section. This includes the name of the SG and its desciptions as well as choosing the VPC where it would apply.
* Then Click Create Security Group.
* 







