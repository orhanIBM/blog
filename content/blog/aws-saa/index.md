---
title: AWS Solutions Architect Associate
date: "2022-01-21T13:50:32.169Z"
description: Just an exam prep notes for AWS Solutions Architect Associate SAA-C02
---

### IAM & AWS CLI
Section 4

#### IAM Users & Groups, Policies
* IAM is a global service
* Root is the default account, do not share it 
* Users are people
* Groups contain only users not other groups (exam question)
* Users do not have to be part of a Group
* Users can be part of more than one Group

* Users OR Groups can be assigned policies, remember single users as well, because some users may not be part of a group
* Policies define permissions
* You apply the least privilege principles 

* Why create a user? To better classify users and assign policies. Not everyone will be a root, just the main person

#### IAM Policies Inheritance
* Users can be assigned to multiple teams and inherit the policies of the teams policies 
* IAM Policies can be an exam question, understand the principal (account/user/role that policy applies), action (get or list, post, delete), resource (aws resource) and effect (allow/deny) and finally Condition, there is no version in the Policy Statements (the JSON document)

#### IAM Password Policy
* Use strong password, set min length + require specific char types
* Require users to change their pw after some time
* Prevent pw re-use 
* MFA - Multi Factor Authentication, recommended 
* What is the main benefit of MFA? Hackers
* [Exam] MFA devices options? Virtual, ie Google Authenticator, Microsoft Authenticator, Authy for multiple tokens, as well as Universal 2nd Factor Security Key such as Ubikey. Hardware Key Fob MFA device is Gemalto and for government *AWS GovCloud US, ther eis a dedicated Hardware Key Fob
* Password Policy setup for IAM users enhances the security
* Other security credentials are Access Keys / CloudFront Key pairs/ X509 Certificates

#### AWS Access Keys, CLI and SDK
* How can users access AWS: AWS Mgmt Console, via CLI (protected by access keys) and finally AWS SDK, used for code and protected by access keys
* Access Keys are secret too, not just Secret Key is secret, so don't share
* Whatever user permission have, it will be the same for the CLI (since you are using the same user credentials) root user's CLI have all the permissions
	
#### AWS CloudShell
* AWS CloudShell allows you to use commands without installing the CLI
* The region is what you selected on the console
* Files created within the cloudshell is persisted for the region
* You can upload / download files into/from cloudshell
	
#### IAM Roles for Services
* IAM Roles for Services, is the Role (set of permissions that can make request) for an AWS resource, such as an EC2 needs permission to perform actions on our behalf on the cluster 
* The permission given to the AWS Resource is called IAM Role for Services
* i.e. Lambda Functions and Roles for CloudFormation are a few examples
	
#### IAM Security Tools
* IAM Credentials Report (account-level): list all accounts and status of their credentials (last password change, MFA availability, etc)
* IAM Access Advisor (user-level): service permissions granted to a user + last access to AWS service + policies granting the permission. To revise user policies you can use Access Advisor report
	
#### IAM Best Practices
* Don't use root other than account setup
* One AWS User = One Physical User
* Assign users to Groups and assign permissions, note Groups cannot be part of other Groups
* Assign Roles to AWS Services for automatic access
* Policies: JSON document outlines permissions for making request to AWS resources, can be used by Users, Groups and Roles
* Create a strong password policy
* Use and enforce MFA
* Use Access Keys for CLI/SDK etc. i.e. Terraform or webapp call
* Audit permissions of an account with the IAM Credentials Report
* Never share IAM users + access keys
	
* [Potential Exam] select AWS security best practices from the list, or which one is not a security best practice 
	
### EC2 Fundamentals
Section 5

#### AWS Budget Setup
* Set up a budget so we don't overpay
* EC2 Basics
* Most popular 
* Elastic Compute Cloud
* EC2 settings: OS, CPU, RAM, Storage (EBS & EFS for Network-attached or directly attached to EC2 hardware called EC2 Instance Store), Network Card (speed of the card and Public IP Address), Firewall Rules and User Data for Bootstrap Script
* EC2 User Data runs as root, so runs as sudo 
* EC2 User Data will be base64 encoded
* A restarted EC2 instance gets a new public IP
	
#### EC2 Instance Types
* General Types, balance between compute, memory, networking, T class
* Compute Optimized: Batch processing, media transcoding, high performance computing, machine learning and dedicated gaming server, choose C class
* Memory Optimized: Large Data Sets in Memory so high performance databases, distributed cache, in memory databases for business intelligence, real time processing of big unstructured data, all require RAM, so choose R class 
* Storage Optimized: High frequency online transaction processing, SQL, MongoDB, Redis, Data Warehousing, Distributed File Systems: choose Input output or Data classes: I or D
	
#### Security Group
* Only Allow Rule, no Deny
* Allow Protocol (TCP, UDP etc), Type (HTTP, TCP), Port # (80, 443) and IP Range (0.0.0.0/0, all or another Security Group)
* If you attach a security group to an EC2 instance (say sg1), and create another EC2 Instance and attach a security group to the new instance with the rule: sg1 is allowed, then the traffic coming from the first EC2 with sg1 can access the second EC2
* Ports to know in the exam
  * 22 SSH
  * 21 FTP
  * 22 SFTP
  * 80 Unsecured Web
  * 443 Secured Web (HTTPS)
  * 3389 RDP - Remote Desktop Protocol for Windows
	
#### EC2 Instance Roles Demo
* When you need your EC2 to execute an action on your behalf, never enter your credentials, instead use IAM Role
* For instance, go to EC2 Connect screen (instead of ssh'ing), and type $aws iam list-users you will get Unable to locate credentials, please configure. But don't instead attach a role, and then re-run the list-users, and then you will get the results
	
#### EC2 Instance Purchasing Options
* [EXAM QUESTION] Pick the cheapest or most appropriate EC2 Option below given a scenario
* On-Demand: Short-workload, predictable pricing
* 1 Year Minimum Reserved Instances:
		○ Reserved: Long Workload
		○ Convertible Reserved Instances: Long workload with the option to change the tier
		○ Scheduled Reserved: every Monday 7am for 1hr (no more available)
* Spot Instances: Short workloads, cheap but can lose instance during work
* Dedicated: Book the entire physical server, you can control the placement
* Scenarios:
* Client wants to use short-term uninterrupted workload, can't predict application behaviour if interrupted, doesn't care about the cost: EC2 On Demand has the highest cost
* 75% discount compared to on-demand, can use it for database for 3 years: Reserved Instance
* Same as above but wants to have the flexibility to upgrade if application scales: Convertible Reserve Instances
* Client wants the cheapest of all solutions: Spot-price up to 90% discount but doesn't care if the process is interrupted, such as data analysis, batch processing, image processing, distributed workload etc: EC2 Spot Instances (spot market vs bid price)
* Help client address compliance requirements and reduce costs by allowing to use existing server-bound software licenses: EC2 Dedicated Hosts, more expensive, you can bring your own license, or due to regulations you cannot share the server
	
#### Spot Instances
* Can get up to 90% discount
* Define the max spot price you wanna pay, if the current price is below of your price, you reserve an instance
* When spot price goes above your max price you have 2 options
		○ Stop/Terminate the instance with a 2 min grace period
		○ Block the spot during a specified timeframe without interruption (not available anymore on AWS, can be in the exam)
* Remember, spot is not good for databases/critical workload
* Spot instances can be launched for 1 time or persistently
* Set the # of instances, types, valid from/to and price then it will add instances when the criteria are met
* To stop spot instances, first stop the setting, then the instances
	
#### Spot Fleets
* What if you want to hit a target capacity under a price constraint? Spot Fleets allow us to automatically request Spot Instances w/ the lowest price
* You can get 1 m5.large or 10 t2.micro both of which can give you the same capacity (let's say), but their  prices are different, how can you optimize your cost? 
* Welcome to Spot Fleets, it pics the spot instance from possible launch pools: instance types, OS, Availability Zones and stops launching more instances when it hits the max cost or capacity
* Strategies to allocate Spot instances:
		○ LowestPrice: use for cost optimization and short workload
		○ Diversified: distribute across all pools, so good for availability + longer workload
		○ capacityOptimized: pool with the optimal capacity for the number of instances
	

### EC2 Solutions Architect Associate Level
Section 6

#### Private vs Public vs Elastic IP
* IPv4 and IPv6 supported
* Companies have private networks 192.168.0.1/22 and can talk to each other
* But as soon as the private network machine needs to go outside world, it goes through an internet gateway, which has a public ip (253.144.135.205)
* So our VPC is private and IG allows machines to connect to internet
* Elastic IP is a static IP, that masks the machines, so if a machine fails, you can replace it and attach the Elastic IP onto it, voila. But that is good for small number of machines
* As the infra grows, a better architecture is Load Balancer and DNS names; will be discussed later
* If you stop / start the instance, the IP address changes as well

#### EC2 Placement Groups
* One of the three: Cluster, Spread, Partition
* When you need low latency, high network throughput or big data job needs to be done fast, put them close to each other: Cluster them on the same AZ (us-east-1a, 1b, 1c), same hardware (rack)
* When you need high availability cuz you have critical applications, put them far from each other, one in each AZ with max 7 instances per group per AZ
* When you use HDFS, Hadoop, Cassandra, Kafka, you need partitions, such that if a rack (hardware) fails, application will go to the next partition. Say you have 3 partitions, p1 goes to us-east-1a rack1, p2 goes to the same AZ but on rack2, and p3 goes to another AZ say us-east-1c on rack1, so if a rack fails, next partition picks up the tab. However, you can have up to 7 partitions per az (i.e. 7 racks in us-east-1a), but also your app can span across multiple AZs in the same region, so in Ohio region, I can have us-east-2a, 2b, 2c, 2d… In each Rack, (partition) you can have 100s of EC2s

	Elastic Network Interfaces
* ENI is a virtual network software, separating the network connection from machines, so that if an EC2 fails, you can just move ENI to a new machine on the fly
* ENIs can have one Private IP, MAC Address, Elastic IP, and Public IP and more Secondary Private IPv4
* You can attach security groups (one or more) to the ENI
* You can attach multiple ENIs to one EC2 Instance (eth0, eth1 etc)
* ENIs are bound to the AZ it is created at, ENI cannot be transferred to another AZ
* Use case: Failovers
	

#### EC2 Hibernate
* If we stop instance data on disk (EBS) is kept intact, terminate: any EBS data is gone
* But either way, data on RAM is gone
* What if the in-memory state is preserved? So that when the instance is booted, we continue where we left off, no need to re-install user data or anything like that
* Requirements: RAM is dumped into a file in the root EBS volume and volume must be encrypted
* RAM size must be less than 150GB (remember GBs of data is put into disk)
* Not supported for bare metal and some other families

#### EC2 - Advanced Concepts
* Nitro - vCPU - Capacity Reservations
* Nitro is the next generation of virtualization technology for better performance, enhanced networking, IPv6 support, higher EBS volumes, 64,000 EBS IOPS on Nitro (exam, higher EBS IOPS, require Nitro)
* vCPU = Thread x Core, but at launch time we can tweak # of Threads or Core Counts to optimize CPU
* For instance, some licenses are tied to CPU Core, so you can increase the thread per core but decrease the number of cores, so you end up with the same vCPU but minimize license cost. Alternatively, if you are high frequency trader that requires HPC, then single thread is faster than multithreading, thus you can say one thread per core pls
* Capacity Reservation: Reserve Capacity but don't tie yourself to 1year or 3year commitments
	
### EC2 Instance Storage
Section 7

#### EBS Overview
* Elastic Block Store
* EBS Volume is a NETWORK drive
* Just like ENI, it is an abstraction of hard disk, so you can attach to your instances
* EBS is LOCKED TO AN AZ BUT CAN BE MOVED ACROSS AFTER A SNAPSHOT
* Since EBS is a network drive, there is a potential latency, compared to an SSD on the machine
* Multiple EBS can be attached to single EC2
* [Exam] Delete on Termination Attribute: by default the root EBS volume is deleted and any other attached EBS volume is not deleted, to preserve data disable deletion


#### EBS Snapshot
* Not a requirement but suggested, detach EBS, snapshot it, and move it to new AZ

#### AMI Overview
* Amazon Machine Image
* Customization of EC2 instance, so start an httpd machine 
* AMI IS REGION SPECIFIC BUT CAN BE COPIED
* Process: create an EC2 instance, customize, stop ec2, build ami (creates ebs snapshot too)

#### EC2 Instance Store
* EBS is a network drive, but if you need high-performance disk, use EC2 Instance Store
* Information in EC2 Instance Store is Ephemeral
* Use case: Buffer / cache / scratch data/ temporary content, remember EPHEMERAL
* Risk of data loss, so user has to backup + replicate

#### EBS Volume Types
* 6 types
		○ GP2 / GP3: SSD balanced, 16TiB, 
		○ IO1/ IO2 SSD: mission critical + low latency / high throughput
		○ ST1 HDD: frequently accessed + throughput intensive
		○ SC1 HDD: less frequently accessed data
* Cost (Cheap to Expensive) sc1, st1, gp, io
* Performance (High to low) io, gp, st, sc
* Should be available as root volume: only gp2/gp3 and io1/io2
* When to use gp2/gp3?
		○ When you need cost effective storage with low latency
		○ When you need to access system boot volumes
		○ When the size is less than 16TiB
		○ Use gp3 when you need to increase iops 16,000 and throughput 1000MiB/s independently
		○ Use gp2 when you don't care, and iops and size are linked
* When to use iops (provisioned iops)
		○ Database workloads, sensitive to storage performance + consistency
		○ IO per second more than 16,000
		○ Or critical business applications with sustained io performance
		○ When you need EBS Multi-attach option
		○ And when you need sub-milisecond latency + 256,000 iops use io2 Block Express
		○ For IO more than 32,000 EC2 should be Nitro
* HDD
		○ Cannot be boot
		○ Min 125MiB 
		○ Big data, data warehouses, log processing: st1
		○ For data that is infrequently used: sc1 
	
#### EBS Multi Attach
* Use case: Achieve high application availability in clustered Linux application
* Applications that must manage concurrent write operations
* Then use io1/io2 family EBS to have multiple EC2 writing into the same volume

#### EBS Encryption
* When you create an encrypted EBS volume:
		○ Encrypted at rest, in flight, snapshots and volumes created out of it
* Encryption has low impact on latency
* Leverages keys from KMS
* Can convert unencrypted EBS to encrypted through a snapshot process
	
#### EFS 
* Elastic File System
* Managed NFS network file system
* Can be mounted to many EC2
* Multi AZ (unlike EBS)
* More expensive compared to gp2, but you pay per use gb
* EFS is only Linux doesn't work on Windows os
* 1000s of concurrent NFS clients can connect to EFS
* Grows to petabyte scale network file system,automatically
* By default EFS is in Performance mode: latency sensitive use cases such as content management, web server, wordpress etc, if you need big data or media processing where latency is not critical but can process in parallel choose Max I/O under performance mode
* After selecting Performance mode, decide on throughput mode, whether it should be a burst mode or constant speed mode, where size of the disk is not a factor
* Finally: Storage Tiers: move file after N days, for Standard: frequently access files and EFS-IA for less frequently accessed data, but when accessed, it costs $ 
	
	
	
	

	
	
	
	
	


