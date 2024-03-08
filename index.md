# SAA-C03 Review Notes:

## Big Hints
- **LEAST Operational Overhead** = Simple, don't create anything new
- **Managed Services** = NOT EC2
- **Low Overhead** = NOT EC2
- **PII** = Macie
- **Unpredictable / Sporadic** = Serverless
- **Pick TWO/THREE** - Pay attention here
- Eliminate obviously wrong options
- Look out for negative questions **NOT**


## Organizations
- Central management of all AWS accounts
- Consolidated billing for all member accounts
- Hierarchical grouping for budgets, security and compliance
- Policies to centralize control over services and resources
- Free service
- Unused Savings Plan in a OU account
	- Create an organization where the savings plan is location
	- Turn on discount sharing in the ORG account billing preferences
- Consolidated billing info on tagged resources
	- Select user-defined tags
	- Activate tags in the management account


## Corporate Accounts
- Enterprise Identity Federation
	- Use STS to support SAML auth
- AWS WorkDocs
	- Create, store, share and manage personal documents
- Active Directory
	- AWS Directory Service AD Connector
	- Join windows isntances to your AD domain
	- Federated sign-in, map AD identities to IAM roles
	- Simple AD
		- Supports basic AD features
		- Linux workloads that need LDAP service
		- No MFA or advanced features
	- Microsoft Active Directory Federation Service (AD FS)
		- Windows Server IPD that supports SAML 2.0
	- Web Identity Federation
		- Sign in with well-known social IDPs
		- Facebook, Google, Amazon


## High Availability
- Ensure apps and DBs are multi-AZ or multi-region
- Scaling Policies


## Disaster Recovery
- Backup & Restore - Hours
- Pilot Light - 10s of minutes
- Warm Standby - Minutes
- Mutli-Site Active-Active - Instant
- Recovery Point Objective (RPO) - How much data can you lose?
- Recovery Time Objective (RTO) - How quickly can you recover?


## General Security
- MOST secure, don't expose credentials = Use IAM roles
	- Associate with EC2 instance profile
- Defense-In-Depth = Keep things private
- GuardDuty - Monitor for malicious activity
- Inspector - Monitor for software vulnerabilities
- Trusted Advisor - Recommendations on a variety of cost, availability and security topics


------------- 


## API Gateway
- API Gateway to ECS - Create a VPC endpoint
- Reduce latency, geo distributed = Edge-optimised endpoint
- Use canary deployments to roll out a new API version
- Use caching to reduce load
- Custom authorizers (JWT, IAM)


## Artifact
- Generate and download compliance reports


## Athena
- Glue Data Catalog Query
- Federated Query
	- Tap into existing relational, non-relational and custom sources
	- Use a connector to tap into the data source


## Certificate Manager
- 3rd party cert, create locally and import to ACM


## CloudFormation
- Use a `CreationPolicy` to signal that a resource is ready
	- Stack creation will proceed after the event


## CloudFront
- Use ACM to create a cert and use DNS validation
- For all static caching scenarios
- Grant access to Cloudfront data
	- Signed cookies - No change to the URL
	- Signed URLs - More control over content access
- Geographic Restrictions
	- Create a list of allowed countries (Allow/Block)
- Cloudfront Functions
- Lambda@Edge
- Authenticate to S3
	- Origin Access Control
		- Permission policy for CloudFront to read data
	- Origin Access Identity (LEGACY)
		- Uses a CF identity as the principal
- SSL Certificates
	- Only one SSL per distribution
	- Use can use a multi-domain certificate from ACM
	- Up to 100 domains are allowed
	- Apply as alternate domain names on the distro


## Control Tower
- Used for managing multiple AWS accounts
- Automated the creation of landing zones
- Implements account best-practices with blueprints
- Extends AWS Organizations


## Cost Explorer
- Visualize, understand and analyz costs and usage
- Cost Explorer API
- Query aggregated data, granular data
- Pagination of large data sets
- Can forecast usage based on past numbers, doesn't require AWS Forecast 


## DocumentDB
- Supports MongoDB compatability
- Document database service
- Highly available, fully managed


## DynamoDB
- Improve response time, decrease latency, least overhead
	- Likely a DAX response
	- From milliseconds to microseconds
- Global Tables
	- Mutli-Region, Multi-Active
	- Read and Write to any replica
- Distribute workloads efficiently
	- Partition keys with high-cardinality, more distinct values
- DynamoDB Streams
	- Ordered flow of information about item changes
	- Capture of every table change
	- Integration with Lambda for event triggers
	- 24h data retention
	- Lambda polls the stream for new data
- Capacity Types
	- Provisioned
		- Capacity must be selected for read and write
		- Too much usage results in throttling
			- `ProvisionedThroughputExceededException`
		- Burst Capacity - Handle capacity that exceeds throughput settings
		- Adaptive Capacity - Increase capacity on hot partitions
	- On-Demand
		- Capacity is automatically determined
	- Auto-Scaling
		- NOT enabled by default
		- You must set initial capacity values
		- Adjusts settings dynamically to handle load
		- Manually adjusted using `UpdateTable` operations


## EC2
- Instance Types:
	- General Purpose
	- Compute Optimized
	- Memory Optimized
	- Storage Optimized
	- Accelerated Computing
	- HPC (High Performance) Sims/Deep Learning
- Scaling at a particular date/time
	- [Auto Scaling Group w/ scheduled action](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html)
- Multiple instances writing to shared storage = EFS
- Savings Plans
	- Compute - Most flexibility
	- EC2 Instance - Select families
	- SageMaker
	- Payments
		- No Upfront
		- Partial Upfront - 50%+
		- All Upfront
- Instance Billing
	- Reserved instances are billed not matter the state
	- On-Demand instances are 
		- Not billed when stopping
		- Not billed when pending
		- Are billed when preparing to hibernate
- Public App - Private DB
	- SG1 - Allow traffic from 0.0.0.0
	- SG2 - Allow traffic from SG1
- Auto-Scaling termination order
	- Select AZ with most instances
	- Select unprotected instances
	- Locate those with the oldest launch template
	- Closest to the next billing hour
	- If multiple options, select at random
- CloudWatch Metrics
	- Network utilization
	- CPU utilization
	- Disk Reads
	- Custom Metrics w/ CloudWatch Agent
		- Memory utilization
		- Disk Swap
		- Disk Space
		- Page files
		- Log Collection
- When an instance is started/stopped
	- The underlying host for the instance can change
	- All instance-store data is lost (ephemeral)
	- Hibernation can be used to save state
		- Can be enabled after an instance is launched
		- Requires space to be allocated on the disk for storage
		- Requires an encrypted EBS volume
	- Prevent EBS root volume deletion
		- `DeleteOnTermination = FALSE`
	- Termination Protection keeps instance from being terminated
- On-Demand Instance Limits
	- vCPU-based, On-Demand Instance limit
	- Submit a request to raise he limit
- Reserved Instances
	- Terminate to prevent on-demand charges after expiration
	- Sell unused instances on the Reserved Instance Marketplace
- Change the AMI being used
	- Create a new launch template
	- Select an updated AMI version


## Elastic Beanstalk
- Supports Apache Tomcat


## Elastic Container Service (ECS)
- Invoke tasks using EventBridge events
- Use a Lambda to trigger StartTask
- Associate a role using `taskRoleArn` in the task definition


## Elastic Kubernetes Service (EKS)
- View insights from EKS and On-Prem = EKS Connector
- EKS Anywhere - Run EKS on your On-Prem
- EKS Secrets, configure to use KMS and Secrets Manager
	- Reference the EKS etcd key-value store
- Can be used with Fargate


## ElastiCache
- Redis
	- Gaming Leaderboards
	- Messaging
	- Streams
	- Authentication
		- --auth-token configuration
		- --transit-encryption-enabled
	- Global Datastore, write and replicate
	- Autoscaling
- Memcached
	- Simplest model
	- Large nodes, multi-core, threads
	- Scale in and out


## EventBridge
- Monitor whether events are being sent = CloudWatch Metrics


## Fargate
- Works with both ECS and Kubernetes
- No need to provision and manage servers
- Application isolation, better security
- Selects the right amount of compute
- Pay for what you use


## Global Accelerator
- Supports TCP/UDP
- Good for streaming services, gaming
- Routes traffic onto AWS network as soon as possible


## Glue ETL
- Create and manage ETL jobs
- Transfer data from S3, RDS, Redshift
- Transform data
- Data Catalog collect and monitors active data
- Crawlers populate the data catalog
	- Can be scheduled or run on-demand
- Bookmarking can be used to keep track of job progress
	- Restarts continue in the correct location
	- Avoids replay of old data


## Identity Access Management (IAM)
- IAM Access Analyzer - Identity Access Issues, Least Privilege
- IAM Identity Center - Manage workforce access to multiple accounts
- IAM Roles Anywhere - Generate temporary credentials for on-premise workloads
- Identity Policies
	- Attach to roles and groups
- Resource Policies
	- Attach to resources
- Users and Groups
- STS - Security Token Service
	- Create temporary security credentials to access resources
	- Role escalation / assume role
	- Cognito and Identity Center are just for Auth


## Key Management Service (KMS)
- Multi-Regions Keys - Replicate keys
	- Can't convert single to multi
- For full control of keys and key material
	- KMS Custom Key Store
		- Use AWS CloudHSM for storage
		- Use an external key store


## Kinesis Streaming
- Will always reference a Kinesis service
	- Data Streams - Capture data from many sources
		- Capacity = On-Demand or Provisioned
		- Default Retention = 24h
		- Shards are scaled automatically
	- Firehose - Capture, transform and load data into storage
	- Data Analytics - Process data with SQL or Flink
- Ingest real-time data, convert using Lambda
	- Kinesis Data Analytics
	- Spark EMR
	- EC2
	- Lambda


## Lake Formation
- Relational DB permissions model for data catalogs
- Define policies and control data access
- Permissions on catalog objects


## Lambda
- Reduce start-up latency, Java runtime = SnapStart
- Use Function URLs to easily expose a function
- Respond to Events to handle a request
- Max runtime is 15 mins
- Permissions must be applied to the execution role
- Can use a Compute Savings Plan
- Can be placed in an existing VPC or default


## Load Balancers
- Application Load Balancer - HTTP/S, WebSocket
	- Can't be assigned an elastic IP
- Gateway Load Balancer - 3rd Party Services, Virtual Appliances (firewalls)
	- Use VPC endpoints
	- Uses the same VPC as the virtual appliance
- Network Load Balancer - TCP/UDP, EC2 and Containers
	- Can be assigned one EIP for each AZ
- SSL Certificates
	- You can upload multiple certs and bind them to one listener
	- The ALB will select the correct cert using Server Name Indication (SNI)


## On-Prem Notes
- Sending data to AWS = Use a Storage Gateway
	- Volume
	- File
	- Tape
- DataSync - copy data to/from AWS
- DirectConnect - Setup a hardline connection to AWS
	- Customer Gateway to DC location
	- Customer Router to AWS DC Router (Cage)
	- DC Router to Virtual Private Gateway
- Site-to-Site VPN
	- On-Prem Customer Gateway
	- VPN Connection to Virtual Private Gateway
- 1 Gbps upload can be considered fast


## Rekognition
- Two types:
	- Rekognition Image
	- Rekognition Video
- Can detect objects, scenes, faces, text, and inappropriate content in videos
- Sentiment analysis
- Related Services
	- Translate = Translate beteen languages
	- Transcribe = Audio to text conversion
	- Comprehend = Language processing, identify meaning and insights
	- Polly = Text to audio
	- Lex = For creating chatbots


## RDS
- TYPES
	- General Purpose
	- Memory Optimized
- Multi-AZ Deployments
	- **Asynchronous** replication for Aurora
	- **Synchronous** replication for Non-Aurora
	- Only primary instance is active
	- Automated backends
	- At least 2 AZs
- Read Replica Deployments
	- **Asynchronous** replication
	- All replicas are accessible
	- No backups by default
	- Manual promotion to primary
- Make the data highly available, increased read capacity
	- Multi-AZ Cluster Deployment
	- Reader endpoint (Read)
		- Automatically load balanced
	- Cluster endpoint (Write)
	- Aurora Custom Endpoints
- On-prem migration solutions
	- Use AWS Migration Service, Compatible DB type
- Need more storage = Enable storage scaling
- Protect Data
	- Encryption
	- Client-Side Encryption
	- Enable IAM DB authentication
- High memory utilization = Memory Optimized Instance
- RDS Proxy 
	- Aurora, MySQL, Postgres, SQLServer, Serverless V2
	- NO Oracle support
	- No app changes
	- Pool and share database connections
- View metrics from Enhanced Monitoring
	- Stored in CloudWatch logs for 30 days
	- Gathered from the instance agent
	- Used for total memory, CPU bandwidth


## RDS Database Migration Service
- Supports Same (Oracle to Oracle) or  (Diff) Oracle to Aurora
- Continuously replicate data
- Stream data to Redshift or S3
- Schema Conversion Tool (SCT)


## RDS Aurora
- Cost optimization, time-boxed usage
	- Use Aurora Serverless MySQL/Postgres
- Unexpected workloads, reduced cost
	- Use Aurora Serverless MySQL/Postgres
- Aurora Auto Scaling
	- Uses a scaling policy with min/max replicas
- Aurora Global Database (READ)
	- Multiple regions
	- Fast local reads
	- Recovery Point Objective < 1s, Recovery Time Objective < 1min
- Aurora Multi-Master (WRITE)
	- Multiple AZs
	- Read and Write
- Invoke a Lambda based on stored procedure or native function
- To Migrate from Aurora to Aurora Serverless
	- Use the AWS Database Migration Service
	- Its not possible to convert from one type to another


## Route 53
- Routing Policies
	- Simple
	- Failover
		- Active-Active
		- Active-Passive
	- Geolocation
		- Use for broader nation controls
	- Geoproximity
		- Use when you need to segment a nation/area
	- Latency
	- IP-Based
	- MultiValue - Many possible IPs
	- Weighted


## Simple Storage Service (S3)
- Store petabytes of data
- Unpredictable Usage / Changing Patterns = Intelligent Tiering
- HIGHLY AVAILABLE = Standard
- No need to access = Glacier Deep Archive
- Glacier Flexible Retrieval
	- Expedited = 1-5 mins
	- Standard - 3-5 hours
	- Bulk = 5-12 hours
- Glacier Deep Archive
	- Standard = up 10 12 hours
	- Bulk = Up to 48 hours
- Transfer Acceleration
	- Upload data to an edge location
- Delete Protection
	- Versioning
	- Object Lock - Prevent deletion or overwrite
		- Requires versioning, automatically enabled
		- Retention Period - How long
			- Compliance Mode - No one can touch it
			- Governance Mode - Some people can touch it
		- Legal Hold - Can be freely removed with PutObjectLegalHold permissions 
	- MFA Delete
- Keep data private
	- Block Public Access
	- ORG service control policy that prevents changes
- Static Website Hosting
	- Bucket name must match the domain name
	- A domain must be registered in Route 53
- S3 Events
	- s3:ObjectCreated
	- s3:ObjectRemoved:Delete - Permanent Deletion
- Faster retrieval
	- Use expedited retrieval 
	- Purchase provisioned retrieval capacity
		- For specific bandwidth requirements
		- Used when you require a guarantee that expedited retrievals are available
		- 1 unit = at least 3 retrievals every 5 minutes
		- up to 150 MB/s of retrieval throughput
- S3 Select to retrieve data subsets
	- Uses simple SQL expressions
	- Requires the bucket and object key


## Snow Family
- Snowcone - 8-14TB
- Snowball - 1-80TB
	- Tape Gateway
- Snowmobile - 100PB


## SNS
- Standard Topic
	- Standard de-duplication
	- Multiple subscription types
- FIFO Topic
	- Strict de-duplication
	- SQS Subscriptions
- Encrypted topics
- Dead Letter Queue using SQS
- SMS Messages
- Mobile Push Notifications
- Email Alerts


## SQS
- Keeps messages up to 14 days
- Message locking
- Message encryption
- Message batches (10 count)
- Messages must be deleted after consuming
	- Otherwise returns after visibility timeout
- Long Polling
- Dead Letter Queue
- Standard Queue
	- At least one
	- Best effort ordering
- FIFO Queue
	- High throughput
	- Exactly once
	- Maintains order


## Storage Solutions
- EBS - Volume attached to an instance
	- Enable encryption be default
	- Can be detached and persist off-instance
	- Supports live configuration changes (type, size, IOPS)
	- GP3: 16k IOPS, 1-16TB, $0.08/GB-mth
	- GP2: 16k IOPS, 1-16TB, $0.10/GB-mth
	- IO2: 256k IOPS, 4-64TB, $0.125/GB-mth
	- IO1: 64k IOPS, 4-16TB,  $0.125/GB-mth
- EFS - File system attached to multiple resources
	- For Linux workloads
	- One-Zone for development use
	- Tiers: Standard, Infrequent Access, Archive
		- Lifecycle Mgmt and Intelligent Tiering
- FSX - High Performance File Systems
	- NetApp ONTAP - NFS, SMB, iSCSI
	- OpenZFS - Linux, Windows, MacOS
	- Windows File Server
		- Active Directory auth
	- Lustre - High Performance, ML, Big Data


## Systems Manager
- AppConfig
	- Configuration management
- Parameter Store 
	- Hierarchical storage for config data and secrets
	- String, SecureString
	- Encryption using KMS
	- Free compared to Secrets Manager
- Fleet Manager
	- Remote management for servers and edge devices
- Compliance
	- Centralized view of infra compliance and performance
- Inventory
	- View of instances and the software installed on them
- Session Manager
	- Access instances without the need for SSH keys
	- No need for bastion hosts
- Patch Manager
	- Deploy OS and software patches to instances


## Transfer Family
- Secure FTP Services
- Move files to S3 or EFS


## Virtual Private Cloud (VPC)
- Connect to DynamoDB or S3 = Gateway Endpoint
	- Otherwise use Interface Endpoint
- Keep EC2 to Dynamo on-network
	- Create a gateway endpoint
	- Create a routing table entry
- Connect privately to service outside of your VPC
	- Use AWS PrivateLink
- Connect TWO VPCs = Peering, Update route table
- Private resources need internet access
	- Create public NAT gateway, route traffic
	- Favor a gateway over a NAT instance
- AWS Network Firewall 
	- Intrusion detection
	- Traffic filtering and monitoring
- Gateway Endpoint Access Control
	- Endpoint policy for trusted S3 buckets
- Not enough subnet IPs?
	- Create a new IPv4 subnet with a larger CIDR range
	- Associate the new subnet with the VPC and launch more instances
- Security Group Connectivity
	- Allow only, no Deny
	- First layer of defense
	- Allow inbound HTTP, SSH etc from user IP
	- Allow outbound to user IP
	- Stateful, remembers the inbound request
		- in via :80, allowed out via :80
	- All rules are evaluated to find a match
- Network ACL Connectivity
	- Second layer of defense
	- Allow inbound on the user IP and correct port
	- Allow outbound from the app port range back to the user IP
	- Stateless, doesn't remember the inbound request
	- Allow AND Deny
	- Rules are evaluated in order
		- First match wins


## VPN (Site-to-Site)
- Requires a Virtual Private Gateway in AWS
- Customer Gateway Device On-Prem
- Customer Gateway Resource in AWS
	- Internet-routable IP address of the Customer Gateway
	- Static or dynamic routing
	- Border Gateway Protocol Autonomous System Number (For Dynamic)
	- (Optional) Private certificate from AWS Private Certificate Authority to authenticate your VPN 
