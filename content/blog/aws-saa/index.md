---
title: AWS Solutions Architect Associate
date: "2022-01-21T13:50:32.169Z"
description: Just an exam prep notes for AWS Solutions Architect Associate SAA-C02, not comprehensive, errors are my own
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

#### EC2 Basics
* Set up a budget so we don't overpay
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
  * Reserved: Long Workload
  * Convertible Reserved Instances: Long workload with the option to change the tier
  * Scheduled Reserved: every Monday 7am for 1hr (no more available)
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
  * Stop/Terminate the instance with a 2 min grace period
  * Block the spot during a specified timeframe without interruption (not available anymore on AWS, can be in the exam)
* Remember, spot is not good for databases/critical workload
* Spot instances can be launched for 1 time or persistently
* Set the # of instances, types, valid from/to and price then it will add instances when the criteria are met
* To stop spot instances, first stop the setting, then the instances
	
#### Spot Fleets
* What if you want to hit a target capacity under a price constraint? Spot Fleets allow us to automatically request Spot Instances w/ the lowest price
* You can get 1 m5.large or 10 t2.micro both of which can give you the same capacity (let's say), but their  prices are different, how can you optimize your cost? 
* Welcome to Spot Fleets, it pics the spot instance from possible launch pools: instance types, OS, Availability Zones and stops launching more instances when it hits the max cost or capacity
* Strategies to allocate Spot instances:
  * LowestPrice: use for cost optimization and short workload
  * Diversified: distribute across all pools, so good for availability + longer workload
  * capacityOptimized: pool with the optimal capacity for the number of instances
	

### EC2 - Solutions Architect Associate Level
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
  * GP2 / GP3: SSD balanced, 16TiB, 
  * IO1/ IO2 SSD: mission critical + low latency / high throughput
  * ST1 HDD: frequently accessed + throughput intensive
  * SC1 HDD: less frequently accessed data
* Cost (Cheap to Expensive) sc1, st1, gp, io
* Performance (High to low) io, gp, st, sc
* Should be available as root volume: only gp2/gp3 and io1/io2
* When to use gp2/gp3?
  * When you need cost effective storage with low latency
  * When you need to access system boot volumes
  * When the size is less than 16TiB
  * Use gp3 when you need to increase iops 16,000 and throughput 1000MiB/s independently
  * Use gp2 when you don't care, and iops and size are linked
* When to use iops (provisioned iops)
  * Database workloads, sensitive to storage performance + consistency
  * IO per second more than 16,000
  * Or critical business applications with sustained io performance
  * When you need EBS Multi-attach option
  * And when you need sub-milisecond latency + 256,000 iops use io2 Block Express
  * For IO more than 32,000 EC2 should be Nitro
* HDD
  * Cannot be boot
  * Min 125MiB 
  * Big data, data warehouses, log processing: st1
  * For data that is infrequently used: sc1 
	
#### EBS Multi Attach
* Use case: Achieve high application availability in clustered Linux application
* Applications that must manage concurrent write operations
* Then use io1/io2 family EBS to have multiple EC2 writing into the same volume

#### EBS Encryption
* When you create an encrypted EBS volume:
  * Encrypted at rest, in flight, snapshots and volumes created out of it
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
* Select Security Group: for allowing efs access, the type is NFS and security group should point to security group attached to EC2
* for EC2, you need to mount the EFS to EC2, see the guide for more
	
#### EBS vs EFS
* Number of instances: EBS-One, EFS-Many
* AZ: EBS-Single AZ, unless snapshot + transfer, EFS-Many
* Termination: EBS-terminated by default, but can be adjusted, EFS-persists
* Price: EFS is more expensive than EBS, but can be cheaper if data amount is small and allow Infrequent Access to save costs

### ELB + ASG: HA and Scalability
Section 8

#### ELB
* Load balancer, like managed nginx
* Why not use nginx directly? Ease of integration with other AWS offerings
* ELB is always HA by default, no need to have a backup ELB
* ELB can do auto healthchecks (but nginx can do it too)
* Old ELB - deprecated: Classic Load Balancer: HTTP/S, TCP, SSL (Secure TCP)
* App Load Balancer: HTTP/S and WebSocket
* Network Load Balancer: TCP, TLS, UDP
* Gateway Load Balancer: Layer 3 load balancer at Network Layer - IP Protocol
* Public ELB: Set Security Group(SG) to allow internet on port 80/443, then EC2 security groups only allowing the SG attached to ELB

#### ALB
* Layer 7 - HTTP/WebSocket grpc
* LB multiple apps on the same EC2 running on containers
* Supports redirects 
* Routing table to different target group, based on url/path/query
* ALB can route to EC2, ECS, Lambda fn or private IP addresses
* ALB gets fixed hostname, xxx.region.elb.amazonaws.com so Route 53 can easily refer to
* EC2 machines behind ALB can get client IP's through X-Forwarded-For header
* Set Rules for path/headers/query string etc for electing target groups

#### NLB
* Layer 4 - TCP/UDP
* Can handle MM request per second
* Latency is lower compared to ALB, so good for gaming
* NLB gets a static IP address, unlike ALB that gets a static hostname, thus you can whitelist an IP address
* To get around the above case, you can assign an Elastic IP to ALB/NLB

#### Gateway Load Balancer
* Layer 3 - IP Packets
* Combines Transperent Network Gateway and Load Balancers
* Uses the GENEVE protocol on port 6081

#### Session Stickiness
* Application based cookies or Duration Based Cookies
* Used for session cookies

#### Cross-Zone Load Balancing
* Allows even distribution amongst the EC2s irrespective of number of EC2 in an AZ
* ALB is always on, no inter AZ transfer fee
* NLB is disabled by default, pay for inter AZ transfer fee

#### SSL Certificates
* ELB uses an X.509 cert
* SNI: Server Name Indication is a header, client should add it to the header
* SNI allows servers to load the correct SSL cert for the correct website on the machine
* SNI solves multi websites on a single host problem
* On AWS only ALB/NLB/CloudFront allows


#### Complete In-Flight Requests
* How to stop EC2 when the connection is still active
* Deregistration Delay feature for ALB/NLB
* If your request/response cycle is fast, set this to low otherwise (photo upload feature) set to high so you drain EC2 properly

#### ASG
* Scale in or out and automatically add EC2s to the target group so ELB can refer to
* ASG refers to average metrics and not the min or max
* Tracking policies: 
	* ASG CPU around 50% : Target Tracking
	* When a CloudWatch alarm is triggered (CPU greater or less than a certain percentage): Simple/Step scaling
	* On Monday 9am: Scheduled scaling
	* Forecast load and schedule ahead: Predictive Scaling
* Scale based on CPU/RAM/Traffic etc

### RDS and ElastiCache
Section 9

#### RDS
* Relational Database Service
* Postgres, MySQL, MariaDB, Oracle, Microsoft SQL, Aurora (AWS Prop)
* Storage can autoscale automatically, good for unpredictable 

#### Read Replicas vs Multi AZ
* Read Replicas, up to 5
* Within the same AZ, Cross AZ or Cross Region
* But since Cross Region is there, it is not real time, thus ASYNC/Eventually consistent
* Once Write fails, Read can be promoted, but needs app configuration (promoted db url)
* Network cost: RDS same replicas within the same region, no fee for cross AZ data transfer, but $$ for Cross Region
* Multi AZ Disaster Recovery: one DNS name, Standby database located in a different AZ is sync in real time, no other access, promoted to Master when needed
* Read Replicas can be set up as Multi AZ for Distaster Recovery too
* Master settings can be modified in real-time to create a standby db in another AZ

#### Encryption
* At rest encrpytion
* Defined at launch time
* [Exam question] if Master is not encrypted, read replicas are not encrpyted as well
* For Oracle / SQL Server there is Transperent Data Encryption
* Encryption through with AWS KMS - AES 256
* In-flight encryption is possible too; you just need to enforce it
* You can also convert un-encrypted db to encrypted db
* IAM policies help who can create/delete dbs, and standard username/password to login the db
* For PostgreSQL and MySQL you can also use IAM based auth, through IAM & RDS API Calls, the password is short lived

#### Amazon Aurora
* Aurora automatically scales out, all the way up to 128TB, i.e. never run out of space
* Aurora can have 15 replicas and fast read replicas
* HA is already built in, so should one db fail, other one picks up automatically 
* 6 copies of your data is on 3 AZ (4 for writes 3 for reads)
* Supports global database version, i.e. data replicated across regions
* Application points to a single DNS name for WRITE, and even if the db fails, aurora promotes a read replica but dns name does not change
* Application points to Reader Endpoint which is a Load Balancer, so you can scale up to 15 reads and don't change the code
* Possibility to create certain endpoints, pointing to larger read db's (i.e. big data analysis reasons or annual reporting etc.)
* Can implement scaling policy to increase the number of read replicas
* Security at Aurora is the same as RDS, so can't ssh, protect with security groups; encrypted at rest, can use IAM etc.

#### Advanced Concepts on Aurora
* If the db workload is intermittent, then choose Aurora Serverless because no capacity planning needed and pay per second used 
* High Availability is a requirement and immediate failover for write node: every node is both read and write
* Aurora Global Database: Useful for disaster recovery but promoting to another region less than 1 minute

#### ElastiCache Overview
* Managed Redis/Memcached
* Note, using ElastiCache requires heavy application code changes
* If not a cache hit, go query the db update elasticache and return the result (depending on cache invalidation strategy)
* A use case is session data stored at ElastiCache
* [Exam] Redis vs Memcached
  * Redis______________________Memcached
  * Multi AZ + auto-failover_____Sharding, multi-node              
  * Data Replicated thus HA____No replication, no HA
  * Data can be durable________No data persistency
  * Backup_____________________No backup
  * Restore____________________No restore

* Why Memcached? Sharding reasons, speed up dynamic web apps
* Why Redis? In-memory data for db cache or message broker
* Elastiache does not support IAM authentication
* IAM policies are only for AWS API level
* Password/token can be created to secure the cache access
* Supports security groups so only certain EC2 can access
* Caching patterns: lazy load, write through, session store
* Gaming leaderboard is an [exam] use case through Redis Sorted sets

### Route 53
Section 10

#### Route 53 Overview
* DNS and domain registrar
* Users can change DNS records to add/remove domain names
* Users can buy domain names
* The only AWS svc that provides 100% availability in the SLA
* Record: How do you want to route traffic to a domain
* Record should contain domain name, record type, value, routing policy and TTL
* [Exam] must know A/AAAA/CNAME/NS record types
* A Record = host name => IPv4 www.orhan.com => 12.34.56.78
* AAAA Record = host name => IPv6
* CNAME = canonical name record, used to alias one domain name to another ie www.orhan.com => www.ibm.com
* Note cannot create CNAME for http://orhan.com, but for http://www.orhan.com or http://blog.orhan.com
* Why not? [See here - freecodecamp](https://www.freecodecamp.org/news/why-cant-a-domain-s-root-be-a-cname-8cbab38e5f5c/)
* NS - Name Servers for the Hosted Zone
* Hosted zones: records of domain and its sub domains
* Public hosted zone: app.example.com
* Private hosted zone: within VPC, app.example.internal
* In a private hosted zone, an EC2 with webapp.acme.internal can ping api.acme.internal and get the local IP of the api server on an another EC2 and send the request. The api server then can try to connect the local db through db.acme.internal and get the local ip of the database and query the db
* TTL: cache the IP address for the duration of the seconds so the dns query is not sent again, high TTL means don't come to the DNS server for a while. If the records are not changing that often, then keep it longer period
* CNAME vs Aliases, remember Canonical Name or CNAME is a pointer to another domain, such as http://api.acme.com => http://ibm.com or to =>http://version2.acme.com; Alias points to an AWS Resources and your root domain http://acme.com can point to an AWS ELB, you cannot achieve that with CNAMEs
* Alias can refer to ELB CloudFront, API Gateway, Elastic Beanstalk, S3 Websites, Global Accelerator, Route 53 Record but not EC2 DNS Name! And cannot set TTL on Alias

#### Route 53 Routing
* Simple: route to single record A/AAAA, for multiple return, random chosen, so cannot associate w/ health checks
* Weighted: Load balancing between regions, A/B testing, 0 weight can be assigned, failover supported
* Failover: (Active-Passive), if health check fails, route the requests to the second record
* Latency Based: Lowest latency to the user from AWS region, failover supported
* Geolocation: Where the user is located, geo IP, used for content restriction
* Geoproximity:  Location based on bias, so on the US map, 90-10 split, so east of Chicago goes to East and west of Chicago goes to West servers
* multi-value
* Private subnet healthcheck since Route 53 cannot access private zone? Create a CloudWatch Metric and associate CW Alarm and Health Check will then check the alarm itself



### Amazon S3
Section 12


### AWS SDK, IAM
Section 13

### S3 and Athena
Section 14

### CloudFront & Global Accelerator
Section 15

### Storage Extras
Section 16

### Decoupling Applications
Section 17

#### Messaging
* Synchronous vs Asynchronous
* Async is mostly event based
* There are ways to decouple applications, queueing, publish/subscribe or real-time stream
* queue: SQS, pub/sub: SNS, real-time stream: Kinesis

#### SQS
* Standard Queue, such as IBM MQ
* Application decoupling in the exam is SQS
* Unlimited throughput and unlimited number of days
* Retention is between 1 min and 14 days
* Latency less than 10ms
* Size 256KB per message
* Standard Queue can have at least once delivery, occasionaly (alternative is FIFO)
* Can have out of order messages, so not orderly
* SendMessage API sends message to SQS Queue, messages is persisted, until a consumer deletes it
* Consumers can be running on EC2/AWS Lamda, on-prem servers
* Consumers Poll SQS messages, up to 10 at a time, then delete message using DeleteMessage API
* SQS Queue + ASG with CloudWatch Metric of Queue Length + Cloud Watch Alarm
* As the EC2 scales up, the same message can be sent to many EC2s, thus remember: at least once, may not be orderly
* Security: at-rest KMS, in-flight HTTPS API, Access Control via IAM to regulate the access to SQS (who can add to/ poll queue), and SQS Access Policies to manage SNS,S3 etc to write to an SQS Queue
* Access Policies are similar to S3; cross account (EC2 in one aws account can poll another account SQS Queue) 
* Another policy is: Publish S3 Event Notifications to SQS Queue, for instance when someone uploads an object to S3 Bucket, S3 Events can be published to SQS Queue and the JSON Policy for that should include Condition [Exam]
* The SQS Queue processing architecture: Visibility Timeout: when a message is being processed by a consumer, that message becomes invisible to other consumers. So no rush case. But if not processed during the timeout, it goes back to the queue, thus can be processed twice; as well, if the consumer did not delete it after processing, it goes back to the queue

* The consumers can also call, ChangeMessageVisibility API to delay the processing time for that specific message 


### Containers
Section 18

#### ECS Overview
* On ECS, the user provisions & maintains the EC2 infrastructure
* Then ECS Service decides where to place the containers
* Two launch types: EC2 Launch Type and Fargate
* Can create an ECS cluster across AZs in a Region
* Multi EC2s register to ASG(s), which are registered to ECS Cluster
* When registered, ECS agent is attached to each EC2s
* Then user can run ECS Tasks such as run httpd server
* Fargate is serverless, does not manage EC2s, no need to manage the infrastructure
* Requirement: Set CPU/RAM requirements and 
* For each Task to run in Fargate, there is an ENI created (one-to-one)
* Since ENI requires private IP addresses, make sure VPC has enough Private IP available for each task
* ECS tasks can interact with other AWS services (DB, SNS etc)
* [Potential Exam Question] EC2 Instance Profile and ECS Task Role
* Remember, in EC2 Launch Profile in ECS, user manages the EC2s; and ECS Agents are attached to the EC2. Therefore EC2 needs to communicate with the ECS service (make api calls), or send logs to CloudWatch service, or pull images from the AWS Registry service ECR, or pull secrets. But to do all of that EC2 needs IAM Permission. That is called EC2 Instance Profile
* But we haven't added any task to the container service. So the permission assigned to the Tasks are ECS Task Role [Another Exam Question]. Make sure that you have a dedicated ECS Task Role for each Task and Task Role is defined in the Task Definition
* Data volumes can be hosted in EFS and EC2 Tasks/Fargate tasks can share data amongs the container. 
* Use Case mutli-az shared storage for containers without managing and infra (i.e. serverless)=> Fargate + EFS
* Load Balancing in EC2 Launch Type: Do not expose a dedicated port, let ECS take care of it. Connect to ALB and allow ALB security group to access any port on the EC2
* For Fargate, since ENI is attached it has unique IP but exposes the same port (say 80), so for security, allow ALB to access the same Task Port on all ENI
* Can automate the tasks via S3 Bucket Event emitting an event to Amazon EventBridge running an ECS Task on the ECS Cluster that has correct ECS Task Role to execute (i.e. get S3 object, transform save in DynamoDB requires ECS Task Role to have S3/DynamoDB access rights)


#### Scaling
* Create CloudWatch Metric (for RAM/CPU useage), triggers CloudWatch Alarm, that scales containers, thanks to ASG
* What if the client is using EC2 Launch type and wants to scale up EC2 size? That is also available through Scale ECS Providers
* Alternative is SQS Queue; create an SQS Queue and ECS that polls messages from SQS, then again create CloudWatch Metric for Queue Length and the rest is the same
* Similar to Kubernetes Rolling Updates, ECS also provides the same functionality

#### ECR
* DockerHub for AWS, supporting ECS + IAM

#### EKS
* Managed Kubernetes Cluster, competing with RedHat Openshift Container Platform

### Serverless
Section 19 & 20

### More Databases
Section 21

### Auditing and Logging
Section 22

### IAM Advanced
Section 23

### Security and Encryption
Section 24

### VPC
Section 25

### Disaster Recovery
Section 26
