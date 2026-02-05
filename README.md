# AWS VIRTUAL PRIVATE CLOUD(VPC) AND NETWORK CONFIGURATION
This documentation outlines the process of creating and configuring an Amazon Web Services (AWS) Virtual Private Cloud (VPC). It covers subnet design, route table configuration, and the implementation of network security using Security Groups and Network Access Control Lists (NACLs). The guide also demonstrates launching an EC2 instance in a public subnet, highlighting best practices for network segmentation, controlled internet access, and layered security within an AWS environment.


## Table Of Contents
1. [Introduction](#introduction)
2. [Pre requisites and Services Used](#pre-requisites-and-services-used)
3. [Planning the IP Address Space](#planning-the-ip-address-space)
4. [Creating the VPC](#creating-the-vpc)
5. [Creating the Subnets](#creating-the-subnets)
6. [Creating and Associating the Route Tables to Subnets](#creating-and-associating-the-route-tables-to-subnets)
7. [Creating an Internet Gateway for the VPC](#creating-an-internet-gateway-for-the-vpc)
8. [Configuring and Associating Security Groups](#configuring-and-associating-security-groups)
9. [Configuring and Associating Network Access Control Lists](#configuring-and-associating-network-access-control-lists)
10. [Creating and Launching an Elastic Cloud Compute on the Frontend Subnet](#creating-and-launching-an-elastic-cloud-compute-on-the-frontend-subnet)
11. [Create Flow Logs](#create-flow-logs)
12. [Validation and Testing](#validation-and-testing)
13. [Common Errors and Troubleshooting](#common-errors-and-troubleshooting)
14. [Conclusion](#Conclusion)

## Introduction
An Amazon Web Services (AWS) Virtual Private Cloud (VPC) is a logically isolated network that enables secure communication between AWS resources, the internet, and on-premises networks. Subnetting refers to the logical division of an IP address range and allows the VPC to be segmented into smaller networks for improved organization, security, and traffic management. Security Groups and Metwork Access Control Lists are network firewalls that filter inbound and outbound traffic preventing unauthorized access. EC2 is a web service provided by AWS that allows you to launch and manage virtual servers, called instances, in the cloud. These instances provide scalable computing capacity, letting you run applications without the need to own physical hardware.

This document outlines the steps required to create an AWS VPC and configure subnets, along with related networking components as well as launching an EC2 instance in the Public Subnet.

## Pre-requisites and Services Used
* Access to AWS Console Dashboard
* An Active Subscription (Free/Paid Tier)
* Designated Availability Zones
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
* On this page, there is a pre-existing destination route rule. Go to Add route, then choose Internet gateway and select the relevant IGW. Then you click Save Changes. 

<p align="center"> <img width="735" height="418" alt="Screenshot 2026-02-03 001009" src="https://github.com/user-attachments/assets/47470ac0-87cb-49aa-95f3-f920a7d0053a" />

## Configuring and Associating Security Groups 
* Select the Security Groups option from the menu section on the VPC dashboard
* Fill in the information in the Basic Details Section. This includes the name of the SG and its desciptions as well as choosing the VPC where it would apply.
* Then Click Create Security Group.
* Next step is to configure inbound and outbound rules which filters network traffic coming and leaving the instance.
* On the security groups page, configure the inbound rules by editing parameters like the type of traffic and inbound source connnections. The protocol and port range is automatic and dependent on the type of      traffic. The source connections could be publicly accessible by either IPv4/IPv6 addresses or configured for private access by you using your IP address.
* Outbound rules are preconfigured to allow all traffic types to all destination addresses (0.0.0.0/0).
* Then Click Save.

<p align="center"> <img width="800" height="450" alt="Screenshot 2026-02-03 220929" src="https://github.com/user-attachments/assets/5800d5b6-b8f9-4029-a12c-c42c9abfcd6b" />


## Configuring and Associating Network Access Control Lists
Creating separate NACLs for each subnet is a best practice that promotes better network structure and clarity. It enables precise control over traffic flow and strengthens security by ensuring that each subnet enforces its own access rules.
* Go to the menu section on the VPC dashboard and click Network Access Control Lists.
* Type in the name as well as tag and select the relevant VPC where it is to apply.
* Then click Create.
* On the NACLS page, head to the Actions tab and click to access a host of functions.
* Select Edit Subnet Associations.
* Check the box for the relevant subnet and click Save Changes.
* The next step requires the configuration of the inbound and outbound rules for the subnet.
* Go to the Frontend NACL page and select Inbound rules. Then click Edit Inbound rules. On this page, we will be required to edit parameters like rule number, type of traffic, protocol and port range, the source   address and the type of action (Allow/Deny) to take place. The following was used; 120: RDP: local machine IP address: Allow action. 
* For the outbound rules, edit the rule number (120), type of traffic (All Traffic), port and protocol range (automatic), the destination address (0.0.0.0/0) and the type of action (Allow).
* Then Save.

<p align="center">  <img width="800" height="450" alt="Screenshot 2026-02-03 142130" src="https://github.com/user-attachments/assets/6d195ca7-22d7-47db-817b-2dc59f4452c3" />

## Creating and Launching an Elastic Cloud Compute (EC2) on the Public Instance
* From the AWS Management Console dashboard, search for EC2 using the search bar and select the service.
* On the EC2 page, select Launch Instance, which takes you to the page where you can configure the instance.
* Type in the name and the tags you intend for use.
* Select the Windows type of Amazon Machine Image (AMI) for your instance. An AMI contains the operating system, application server, and applications for your instance. There are other variations if needed.
* Choose the most suitable instance type that is ideal for the project. An instance type specifies the compute resources assigned to an EC2 instance, including CPU, memory, storage, and networking performance.
* Create a new key pair. it is used to securely connect to your instance. Click the Create New Key Pair which redirects you to page where you type the name, choose the key pair type (RSA) and the private key       file format (.pem). Then create. The file will be downloaded to your local machine.
* Set up the Network Settings in the Instance. This requires you to select the VPC and the subnet (Frontend) where this instance would be added.
* Enable the option to auto-assign public IP address to the instance.
* Select the existing Frontend security group created in the VPC and assign 
* Configure the storage for the instance. In this step, storage is assigned to the EC2 instance by configuring the root volume. A 30 GiB gp3 volume is selected, providing 3,000 IOPS(Input and Output Operations     per Second) for balanced performance. This storage will be used by the instance’s operating system and files.
* Review your configurations and Click Launch Instance.

<p align="center"> <img width="800" height="450" alt="Screenshot 2026-02-03 171009" src="https://github.com/user-attachments/assets/84653be7-ce17-4c84-a01a-51940c52b0ca" />


## Create Flow Logs
Flow logs are used to record network traffic going in and out of your VPC, subnets, or network interfaces. They help you monitor, troubleshoot, and analyze network activity.
* On the VPC overview page, sselect your VPC by checking the appropriate box.
* Navigate to a menu section in a row format just below your VPC list and select Flow Logs. Then click Create FLow Log.
* On the FLow Log page, fill in the name in its section.
* Choose the type of traffic you want it to capture (accepted traffic only, rejected traffic only, or all traffic).
* Select the maximum interval of time during which a flow of packets is captured and aggregated into a flow log record. This could be 1 minute or 10 minutes.
* Then select the destination where this logs will be published. For this, an S3 bucket will be created.
* After S3 bucket has been created, select it and copy its ARN (Amazon Resource Name). Paste in the S3 bcuket ARN section to define the destination where logs generated would be stored.
* Check the box to include Amazon ECS metadata. 
* Leave the rest in defualts and click Create Flow Logs.
* You can create multiple flow logs if needed. 

<p align="center"> <img width="800" height="450" alt="Screenshot 2026-02-03 142440" src="https://github.com/user-attachments/assets/64d981d9-df91-4919-aee1-1098f658f652" />

## Validation and Testing

All resources, including EC2 instances, subnets, flow logs, internet gateway, network ACLs, and security groups, have been created successfully. Validation and testing are carried out to confirm proper connectivity, correct application of security rules, and accurate traffic logging. This ensures the environment is secure and functioning as intended.
* On the EC2 dashboard, navigate to and click the Connect tab.
* The next page shows 3 options that can be used to connect to an instance through a browser based client. Select RDP Client.
* Choose the connection type- Connect using RDP client and download the remote desktop protocol shortcut file. You may also use your local machine’s built‑in Remote Desktop (RDP) client to connect to the           instance.
* Generate your password by uploading and decrypting the key pair downloaded earlier.
* Access your instance by using the built-in RDP client on your local machine. Enter your instance’s IP address and your password to log in.
* You can also use a downloaded RDP client to do the same—simply log in using the provided username and password.
* The EC2 instance can now be accessed successfully, confirming that all configurations are working as expected and the connection is stable.

## Common Errors and Troubleshooting
* NACL and Security Group Configuration Errors – Some rules initially blocked required traffic. This was resolved by carefully reviewing and correcting inbound and outbound rules to allow necessary protocols and   ports.
* Availability Zone Mismatch During Subnet Creation – The subnet was initially created in a different availability zone than intended. This was resolved by verifying and recreating resources in the correct         availability zone.

Issues were resolved through step-by-step troubleshooting, including checking network configurations, validating security settings, and testing connectivity after each change.

## Conclusion
This project demonstrates the successful configuration and deployment of a cloud instance with the required networking and security settings. The instance is properly associated with its subnet and security group, allowing secure remote access through RDP using the assigned public IP address. This setup confirms proper connectivity, access control, and operational readiness of the instance within the cloud environment.


