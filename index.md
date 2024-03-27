# SAA-C03 Review Notes:


## Big Hints
- **LEAST Operational Overhead** = Simple, don't create anything new
- **Managed Services** = NOT EC2
- **Low Overhead** = NOT EC2
- **PII** = Macie
- **Unpredictable / Sporadic** = Serverless
- **Pick TWO/THREE** - Pay attention here
- Eliminate obviously wrong options

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
- Service Control Policies (SCP)
	- Do not affect service-linked roles
		- Role is linked directly to the service
		- Predefined and include all needed permissions
	- Overrides local IAM roles
	- Even affects the root user

## Corporate Accounts
- Enterprise Identity Federation
	- Use STS to support SAML auth
	- Configure IAM roles and policies to grant access levels
- AWS WorkDocs
	- Create, store, share and manage personal documents
- Active Directory
	- AWS Directory Service AD Connector
	- Join windows instances to your AD domain
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
- Cost Monitoring
	- Use AWS Cost Anomaly Detection
	- Create cost monitors and get alerts
	- Analyze the root causes of spending increases

## High Availability
- Ensure apps and DBs are multi-AZ or multi-region
- Scaling Policies

## Disaster Recovery
- Backup & Restore - Hours
- Pilot Light - 10s of minutes
- Warm Standby - A few minutes
- Mutli-Site Active-Active - Instant
- Recovery Point Objective (RPO) - How much data can you lose?
- Recovery Time Objective (RTO) - How quickly can you recover?

## General Security
- MOST secure, don't expose credentials = Use IAM roles
	- Associate with EC2 instance profile
- Defense-In-Depth = Keep things private
- GuardDuty - Monitor for malicious activity
- Inspector - Monitor for software vulnerabilities
- Trusted Advisor - recommendations on a variety of cost, availability and security topics

------------- 

## AMI Creation
- Can copy across regions
- Encrypted AMI snapshot = encrypted copy snapshot
- AMIs can be shared across accounts

## API Gateway
- API Gateway to ECS - Create a VPC endpoint
- Reduce latency, geo distributed = Edge-optimised endpoint
- Use canary deployments to roll out a new API version

## Artifact
- Generate and download compliance reports

## Athena
- Serverless, fully managed
- Use Glue to automatically crawl data sources, populate your catalog
- Use SQL queries on data in S3
- Restrict data access with bucket policies and ACLs
- Can invoke SageMaker models in a query
- Athena supports Apache Spark
- Federated Query
	- Tap into existing relational, non-relational and custom sources
	- Use a connector to tap into the data source

## AppSync
- Built for frontend web and mobile applications
- Publish real-time data updates to your application
- Uses GraphQL for data exchange
	- Uses pipeline resolvers to map multiple sources to a field response
	- Perform an auth check before returning data
	- Before and After mapping, Request/Response mapping
	- Mapping templates can be written in Apache Velocity Template language (VTL)
- Fully-managed server-side caching
- Offline data caching for apps
- Fine-grained access control to data
- Custom domain names can be applied
- Consider AppSync when GraphQL is mentioned

## Auto Scaling Groups
- Logical grouping of EC2 instances
- Continues to maintain a fixed number of instances even if an instance becomes unhealthy
- If an instance becomes unhealthy, the group terminates the unhealthy instance and launches another instance to replace it
- Launch On-Demand Instances, Spot Instances, or both
- If you specified multiple Availability Zones, the desired capacity is distributed across these Availability Zones
- Create groups using Launch Templates
	- Previously used Launch Configurations (OLD)
- An instance maintenance policy helps you minimize any potential disruptions
	- Make sure that the ASG first launches a new instance and then waits for it to be fully ready before terminating the unhealthy instance
- Lifecycle Hooks
	- Instance launching
	- Instance terminating
- Warm Pools
	- For instances will long boot up times
	- Pre-initialized EC2 instances that sits alongside an ASG
	- Draw on the warm pool to meet its new desired capacity
	- States: Stopped, Running, Hibernating
- Health Checks
	- Check status of instances
- Events
	- SNS
	- EventBridge
- Unbalanced Resources
	- Happens when:
		- Change the group AZ
		- Terminate or detach instances
		- Insufficient capacity
		- Changes in spot pricing
	- ASG will compensate by rebalancing
	- Launches new instances before terminating old
	- ASG creates a new scaling activity for terminating unhealthy instances, then terminates it
## Batch
- Batch processing for ML model training, simulations and analysis at scale
- Fully managed
- Supports auto-scaling across multiple instances

## Batch Processing Tasks
- Spark / Hive / HBase / Flink = EMR
- Big Data Analysis
	- Use EMR or Glue

## Certificate Manager
- 3rd party cert, create locally and import to ACM
- Monitor for expiring certificates
	- Use EventBridge to check for AWS Health or ACM Expiration Events
	- Use a scheduled EventBridge rule to check the `DaysToExpiry` metric of your certs
	- THEN - Send a notification via SNS for interested parties

## CloudFormation
- Use a `CreationPolicy` to signal that a resource is ready
	- Stack creation will proceed after the event
- Use StackSets to deploy the sample template across multiple accounts

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
- Security
	- Signed URLs or Cookies
- Geographic Restrictions
	- Uses a 3rd party IP database
- Field-Level Encryption
	- Securely upload sensitive information to your web servers
	- The sensitive information provided by your users is encrypted at the edge
	- Only the target origin can decrypt and see the data
- SSL Certificates
	- Only one SSL per distribution
	- Use can use a multi-domain certificate from ACM
	- Up to 100 domains are allowed
	- Apply as alternate domain names on the distro


## CloudTrail
- Log File Validation
	- Ensure that logs have not been tampered with
	- Added file security
- CloudTrail Lake can be used to run SQL-like queries on event logs
	- Directly analyze log files

## CloudWatch
- Container Insights, Lambda Insights, Application Insights
	- Continuously analyze data
	- Powered by Sagemaker, automated dashboards that show potential problems
	- Recommends and configures metrics
	- Reports on failed backups, memory leaks, large requests, cancelled operations

## Compute Optimizer
- Recommends optimal AWS resources for your workloads
- Works with EC2, EC2 Auto-Scaling Groups, EBS, Lambda Functions

## Control Tower
- Used for managing multiple AWS accounts
- Automated the creation of landing zones
	- Automate account provisioning
	- Automate account closures
	- Configure regions
- Implements account best-practices with blueprints
- Extends AWS Organizations
- Automatically detects drift in accounts
	- Account drift notifications
	- Sends notifications using SNS

## Cost Explorer
- Visualize, understand and analyze costs and usage
- Cost Explorer API
- Query aggregated data, granular data
- Pagination of large data sets
- Can forecast usage based on past numbers, doesn't require AWS Forecast 
- AWS Billing and Cost Management
	- Create cost monitors with Cost Anomaly Detection

## DocumentDB
- Supports **MongoDB** compatibility
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
- Point-In-Time Recovery (PITR)
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
	- General Purpose - Balance of compute, memory and storage
	- Compute Optimized - High performance processors
	- Memory Optimized - Faster in memory performance
	- Storage Optimized - High sequential access to large data sets
	- Accelerated Computing
	- HPC (High Performance) Sims/Deep Learning
- Scheduling
	- Use an instance scheduler to control when an instances runs
- Auto Scaling Groups
	- Scheduled Scaling
		- [Auto Scaling Group w/ scheduled action](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html)
	- Simple Scaling
		- Select the min and max instances
		- Increase group size / decrease group size
		- When alarm, add X instances then wait for X seconds before next action (cooldown)
	- Step Scaling
		- Scale by X instances for each metric value
		- Use warm-up to prevent scaling from being too reactive
	- Target Tracking
		- Specify a target metric value (avg cpu utilization, throughput, etc)
		- Define a warm-up time
		- Instances will scale until the metric is matched
- Auto-Scaling termination order
	- Select AZ with most instances
	- Select unprotected instances
	- Locate those with the oldest launch template
	- Closest to the next billing hour
	- If multiple options, select at random
- Launch Templates
	- If the template or VPC defaults to dedicated, its always dedicated
- Placement Groups
	- Cluster - Packs instances closely together within a single AZ
	- Partition - Spread out to reduce the likelihood of correlated hardware failures (Cassandra / Hadoop / Kafka)
	- Spread - Strict placement in small groups, distinct hardware to prevent failures
		- No more than 7 instances per rack
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
	- Reserved instances are billed no matter the state
		- You reserved them, you're paying for them
	- On-Demand instances are 
		- Not billed when stopping
		- Not billed when pending
		- Are billed when preparing to hibernate
- Reserved Instances
	- Regional or Zonal reservations
	- Standard or Convertible
		- Standard can be sold on marketplace
		- Convertible cannot be sold
		- Convertible can be exchanged for other types
	- On-Demand Capacity Reservations
		- Reserve compute capacity
- Spot Instances
	- Spare EC2 capacity up to 90% off
	- Interrupted by AWS with 2min notice
	- Spot fleet can consist of spot and on-demand instances for target capacity
- Public App - Private DB
	- SG1 - Allow traffic from 0.0.0.0
	- SG2 - Allow traffic from SG1
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
		- This can only be changed by using the CLI
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
- Enhanced Networking
	- Elastic Network Adapter
		- Enables high throughput and packet per second performance
		- Consistent low latency on EC2 instances
		- Supports Windows
	- Elastic Fabric Adapter
		- Network device to support High Performance Computing (HPC) and ML
		- Provides lower and more consistent latency, higher throughput
		- Better inter-instance communication rates
		- Not supported for Windows
- Tenancy of a host can be changed
	- Dedicated to Host
	- Host to Dedicated
	- By default they use Shared tenancy
- EC2 Recovery
	- A recovered instance is identical to the original instance
		- including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata
	- If your instance has a public IPv4 address, it retains the public IPv4 address after recovery
- Running a single instance with auto recovery
	- ASG with min=1, max=1, desired=1
	- Attach elastic IP
	- Assign EC2 instance role
- Communicate via private IP to stay in network

## Elastic Beanstalk
- Docker, Go, Java, .NET, Node, PHP, Python and Ruby
- Supports Apache Tomcat and Java SE
- Supports Dockerized Applications
- Provides auto scaling, load balancing and health monitoring by default
	- This must all be enabled for ECS

## Elastic Container Service (ECS)
- Fully managed orchestration service
- Integrated with ECR for container management
- Can run on: 
	- Managed EC2 instances
	- Fargate (Serverless)
	- On-Prem VMs
- ECS cluster is a logical grouping of tasks or services
- Invoke tasks using EventBridge events (Easier Option)
- Use a Lambda to trigger StartTask
- Associate a role using `taskRoleArn` in the task definition

## Elastic Kubernetes Service (EKS)
- View insights from EKS and On-Prem = EKS Connector
- EKS Anywhere - Run EKS on your On-Prem
- EKS Secrets, configure to use KMS and Secrets Manager
	- Reference the EKS etcd key-value store
- Can be used with Fargate
- EKS Scaling
	- Kubernetes Metrics Server is an aggregator of resource usage data in your cluster
		- Not installed by default
		- Commonly used by the Horizontal Autoscaler, Kubernetes Dashboard
	- Kubernetes Horizontal Pod Autoscaler automatically scales the number of Pods in a deployment
	- Amazon EKS supports two autoscaling products:
		- Karpenter
		- Cluster Autoscaler

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
	- Large nodes, multi-core, multithreaded
	- Scale in and out

## EventBridge
- Monitor whether events are being sent = CloudWatch Metrics

## Fargate
- Works with both ECS and Kubernetes
- No need to provision and manage servers
- Application isolation, better security
- Selects the right amount of compute
- Pay for what you use

## Firewall Manager
-  Centrally configure and manage firewall rules across your accounts and applications in AWS Organizations
- Works with:
	- AWS WAF
	- AWS Shield Advanced
	- VPC security groups
	- AWS Network Firewalls
	- Route 53 Resolver DNS Firewall rules
- Does not work with Network ACLs

## Global Accelerator
- Supports TCP/UDP
- Good for streaming services, gaming
- Routes traffic onto AWS network as soon as possible
- Supports uploads and downloads
- Provides static IP addresses
	- Avoid making client-facing IP changes or DNS updates
	- Allocates two IPv4 addresses
	- Bring Your Own IP (BYOIP)
- Fine-grained traffic control
- DDoS resiliency at the edge
- Client affinity (sticky sessions)
- S3 Multi-Region Access Points
	- Routes traffic to data for lowest latency

## Glue ETL
- Create and manage ETL jobs
- Transfer data from S3, RDS, Redshift
- Transform data
- Manage and enforce data schemas
- Can dedupe and cleanse data using ML
- Developer endpoints for testing ETL code
- Glue DataBrew to visualize, clean and normalize data
- Glue Sensitive Data Detection helps you define, identify, and process sensitive data in your data pipeline and data lake
- Auto scale to match workloads
	- Add and remove workers automatically
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
- Users and Groups
- Identity Policies
	- Attach to roles and groups
- Resource Policies
	- Attach to resources
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
	- Video Streams
	- Data Streams - Capture data from many sources
		- Capacity = On-Demand or Provisioned
		- Default Retention = 24h
		- Shards are scaled automatically
	- Firehose - Capture, transform and load data into storage
	- Data Analytics - Process data with SQL or Flink
- Ingest real-time data, convert using Lambda
	- Kinesis Data Analytics
		- Process and analyze streaming data using standard SQL
		- Generate time-series data
		- Feed real-time dashboards
		- Create real-time metrics
	- Spark EMR
	- EC2
	- Lambda

## Lake Formation
- Relational DB permissions model for data catalogs
- Define policies and control data access
- Permissions can be defined on catalog objects
- Share data with other accounts using tag-based access control
	- Can also use names resources

## Lambda
- Reduce start-up latency, Java runtime = SnapStart
- Use Function URLs to easily expose a function
- Respond to Events to handle a request
- Max runtime is 15 mins
- Permissions must be applied to the execution role
- Can use a Compute Savings Plan
- Can be placed in an existing VPC or default
- Encrypt environment variables using KMS
	- Enable encryption helpers to encrypt in transit

## Load Balancers
- Application Load Balancer - HTTP/S, WebSocket
	- Can't be assigned an elastic IP
	- Use to handle multiple domains with dynamic port mapping
- Gateway Load Balancer - 3rd Party Services, Virtual Appliances (firewalls)
	- Use VPC endpoints
	- Uses the same VPC as the virtual appliance
- Network Load Balancer - TCP/UDP, EC2 and Containers
	- Can be assigned one EIP for each AZ
	- Can use HTTP health checks to detect issues
- SSL Certificates
	- You can upload multiple certs and bind them to one listener
	- The ALB will select the correct cert using Server Name Indication (SNI)
- Traffic from an ELB is routed to an instances private IP
- Cross-Zone Load Balancing
	- Equally divides traffic across all registered instances
	- Disabled, equally divides across targets (AZ1, AZ2, etc)

## Neptune
- Graph database tracking relationships between nodes
- Neptune streams can be used to track database changes

## On-Prem Notes
- Sending data to AWS = Use a Storage Gateway
	- Volume - iSCSI protocol, saved as EBS snapshots
	- File - Store files as objects on S3
	- FSx File - SMB protocol, Store files on FSx
	- Tape - iSCSI-based virtual tape storage, saved to S3
- DataSync - copy data to/from AWS
- DirectConnect - Setup a hardline connection to AWS
	- Customer Gateway to DC location
	- Customer Router to AWS DC Router (Cage)
	- DC Router to Virtual Private Gateway
- Site-to-Site VPN
	- On-Prem Customer Gateway
	- VPN Connection to Virtual Private Gateway
- 1 Gbps upload can be considered fast

## Proton
- Supports automated deployments of microservices
- Uses templates to create standards for developer use
- Create stack templates including CI/CD pipelines
- Customer managed environments

## RedShift
- Fully-managed petabyte scale data warehouse
- Extends data warehouse queries to your data lake and against S3
- OLAP type database
- Single AZ deployments
- Massive parallel processing
- Uses machine learning to deliver high throughput
- Uses a cluster of nodes, scalable
	- Dense storage nodes
	- Dense compute nodes
- Redshift Spectrum
	- Run queries against exabytes of S3 data
	- Enhanced VPC routing
	- Only scans the columns needed
	- Can use compressed data in supported formats
- Can ingest data from Kinesis or Kafka streams
- Migrate databases to Redshift using Database Migration Service
	- Easier than using Glue, significant dev effort
	- Zero-ETL service

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
	- Increased availability during OS patching and instance scaling
- Read Replica Deployments
	- **Asynchronous** replication
	- All replicas are accessible
	- No backups by default
	- Manual promotion to primary
	- You can use a replica, but not a standby
	- Should have the same compute/storage as primary to operate effectively
- Make the data highly available, increased read capacity
	- Multi-AZ Cluster Deployment
		- Semi-synchronous, high availability deployment mode
		- Two readable replica DB instances
		- A writer DB instance and two reader DB instances in three separate Availability Zones
		- Provide high availability, increased capacity for read workloads, and lower write latency when compared to Multi-AZ DB instance deployments
	- Reader endpoint (Read)
		- Automatically load balanced
	- Cluster endpoint (Write)
	- Aurora Custom Endpoints
- On-Prem migration solutions
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
	- Enable enhanced monitoring
	- Stored in CloudWatch logs for 30 days
	- Gathered from the instance agent
	- Used for total memory, CPU bandwidth
	- CloudWatch only provides basic metrics on RDS
- Engine maintenance
	- Primary and standby are upgraded at the same time
	- Will result in downtime during maintenance window
- OS Maintenance
	- Secondary is upgraded first, then primary
	- Only downtime is during failover

## RDS Database Migration Service
- Supports Same (Oracle to Oracle) or  (Diff) Oracle to Aurora
- Full load and capture
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
- Failover is handled automatically
	- If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME)
	- If you are running Aurora Serverless and the DB instance or AZ becomes unavailable, Aurora will automatically recreate the DB instance in a different AZ
	- If you do not have an Amazon Aurora Replica, not running Aurora Serverless
		- Aurora will attempt to create a new DB Instance in the same Availability Zone as the original instance
		- Aurora is designed to recover as quickly as possible, and the fastest path to recovery is often to restart or to fail over to the same DB instance. 
		- Restarting is faster and involves less overhead than failover.

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
- Use an alias to add www for a domain
	- Cant create a CNAME for a top node of the DNS namespace
- Route from one domain to another
	- Use a CNAME record
- R53 Resolvers
	- Inbound and Outbound endpoints
	- Route traffic to and from DNS resolvers
- Private Hosted Zones
	- Enable DNS settings in the VPC you want to support
		- enableDnsHostnames
		- enableDnsSupport
	- VPC must be in the same account as the hosted zone
	- Create Hosted Zone > Create Private Hosted Zone
		- Type = Private Hosted Zone
		- Enter the VPC ID to associate

## Service Quotas
- Check AWS Trusted Advisor Service Limits 
- Use a function to refresh service limit checks, update daily
- Capture status events from EventBridge and check limits

## Simple Storage Service (S3)
- Store petabytes of data
	- 3500 requests-per-sec upload
	- 5500 requests-per-sec retrieval
- Encryption
	- Server-side encryption with Amazon S3 managed keys (SSE-S3)
		- Each object is encrypted with a unique key
		- All Amazon S3 buckets have encryption configured by default
	- Server-side encryption with AWS Key Management Service (AWS KMS) keys (SSE-KMS)
		- You can create and manage customer managed keys
		- Use AWS managed keys that are unique to you, your service, and your Region
	- Dual-layer server-side encryption with AWS Key Management Service (AWS KMS) keys (DSSE-KMS)
		- Similar to KMS
		- Applies two individual layers of object-level encryption instead of one layer
	- Server-side encryption with customer-provided keys (SSE-C)
		- You manage the encryption keys
		- Amazon S3 manages the encryption as it writes to disks and the decryption when you access your objects
- Unpredictable Usage / Changing Patterns = Intelligent Tiering
- HIGHLY AVAILABLE = Standard
- Standard-IA (Infrequent Access)
	- Infrequent data access
	- Object must be 30 days old to apply
- One-Zone-IA (One AZ, Infrequent Access)
	- For Recreatable Data
	- Infrequent data access
	-  Object must be 30 days old to apply
- No need to access = Glacier Deep Archive
- Glacier Flexible Retrieval
	- Expedited = 1-5 mins
	- Standard - 3-5 hours
	- Bulk = 5-12 hours
	- 90 days min storage
- Glacier Deep Archive
	- Standard = up to 12 hours
	- Bulk = Up to 48 hours
	- Object must be 180 days old to apply
	- DataSync can move objects directly into a Deep Archive
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
	- URL formats:
		- `bucket-name`.s3-website-`Region`.amazonaws.com
		- `bucket-name`.s3-website.`Region`.amazonaws.com
	- A domain must be registered in Route 53
	- Bucket name must be the same as the domain name
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
- Byte Range Fetch
	- Using the Range HTTP header in a GET Object request
	- You can fetch a byte-range from an object, transferring only the specified portion
- Server Access Logging
	- Get detailed information on all requests and bucket activity
- Secure connection from CloudFront, use OAI Origin Access Identity

## Simple Workflow Service (SWF)
- Build, run, and scale background jobs that have parallel or sequential steps
- Coordinate work across distributed components, tracking the state of tasks
- You implement _workers_ to perform tasks. 
- Workers can run either on cloud infrastructure, such as Amazon Elastic Compute Cloud (Amazon EC2), or on your own premises

## Snow Family
- Snowcone - 8-14TB
- Snowball - 1-80TB
	- Tape Gateway
- Snowmobile - 100PB
	- Data less than 10PB, use Snowball

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
- Scale instances using target tracking from SQS metrics
- Long Polling
- Dead Letter Queue
- Delay Queues can postpone message delivery
	- 0s to 15min
- Standard Queue
	- At least one
	- Best effort ordering
- FIFO Queue
	- High throughput
	- Exactly once
	- Maintains order
	- 3000 messages per second
	- Name must end in .fifo suffix
- Temporary Queue
	- Use for development purposes
	- Lightweight comms channels
	- Create/delete without added cost

## Storage Solutions
- Instance Store
	- Can't detach and reattach to a different instance
	- Data isn't persisted to an AMI created from an instance
	- When you stop, hibernate, or terminate an instance, every block of storage in the instance store is reset
	- You can specify instance store volumes for an instance only when you launch it.
	- ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content
	- SSD instances store volumes for high IOPS performance
- EBS - Volume attached to an instance
	- Enable encryption by default in the account
		- Snapshots will then be automatically encrypted
		- All data moving between the volume and instance are encrypted
	- Can be detached and persist off-instance
	- Can be used while a snapshot is in progress
	- Supports live configuration changes (type, size, IOPS)
	- GP3: 16k IOPS, 1-16TB, $0.08/GB-mth
	- GP2: 16k IOPS, 1-16TB, $0.10/GB-mth
	- IO2: 256k IOPS, 4-64TB, $0.125/GB-mth
	- IO1: 64k IOPS, 4-16TB,  $0.125/GB-mth
	- Multi-Attach Volumes
		- Attach a single Provisioned IOPS SSD (io1 or io2) volume to multiple instances that are in the same Availability Zone
		- Multi-Attach is supported exclusively on Provisioned IOPS SSD volumes
- EFS - File system attached to multiple resources
	- For Linux workloads
	- One-Zone for development use
	- Tiers: Standard, Infrequent Access, Archive
		- Lifecycle Mgmt and Intelligent Tiering
		- Max age for EFS files in 90 days before using Infrequent Access
	- Performance Modes
		- General Purpose - lowest per-operation latency and is the default performance mode for file systems
		- Max IO - previous generation performance type that is designed for highly parallelized workloads that can tolerate higher latencies than the General Purpose mode
	- Throughput Modes
		- Elastic - when you have spiky or unpredictable workloads and performance requirements
		- Provisioned - if you know your workload's performance requirements, or when your application drives throughput at an average-to-peak ratio of 5% or more
		- Bursting - when you want throughput that scales with the amount of storage in your file system
- FSX - High Performance File Systems
	- NetApp ONTAP 
		- NFS, SMB, iSCSI 
		- When multiple protocols are needed
		- Sync data using NetApp SnapMirror
	- OpenZFS - Linux, Windows, MacOS
	- Windows File Server
		- Active Directory auth
	- Lustre - High Performance, ML, Big Data
		- Scratch File System - For temp data, high burst throughput
		- Persistent File System - For stored data, throughput focused

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
- Run Command
	- Remotely and securely manage isntance configuration
	- Access managed EC2 or On-Prem instances
	- Automate common administrative tasks and perform one-time configuration changes at scale

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
		- Route Table 0.0.0.0/0 -> Your Internet Gateway
	- Egress-Only Internet Gateway for IPv6
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
	- Supports
		- Single IP V4 and V6
		- CIDR block
		- Prefix list
		- Another security group
- Network ACL Connectivity
	- Second layer of defense
	- Allow inbound on the user IP and correct port
	- Allow outbound from the app port range back to the user IP
	- Stateless, doesn't remember the inbound request
	- Allow AND Deny
	- Rules are evaluated in order
		- First match wins
- Customer Managed Prefix Lists
	- A list of CIDR blocks to be applied to all security groups
	- Shared lists using Resource Access Manager (RAM)
- Connecting to the internet?
	- Private outbound - NAT Gateway
		- Support IPV4 and IPV6
	  Public outbound - Internet Gateway
  - Security Groups
	  - Can be associated with a NAT instance
	  - NAT instance can be used as a bastion server
	  - NAT instance supports port forwarding
	  - Not associated with NAT Gateways
		  - No port forwarding
		  - No bastion capabilities
  - VPC Sharing
	  - Share one or more subnets with other AWS accounts in the same ORG

## VPN (Site-to-Site)
- Requires a Virtual Private Gateway in AWS
- Customer Gateway Device On-Prem
- Customer Gateway Resource in AWS
	- Internet-routable IP address of the Customer Gateway
	- Static or dynamic routing
	- Border Gateway Protocol Autonomous System Number (For Dynamic)
	- (Optional) Private certificate from AWS Private Certificate Authority to authenticate your VPN 

## Wavelength
- Embeds AWS compute and storage services within 5G networks
- Edge computing infrastructure for developing, deploying, and scaling ultra-low-latency applications
- Wavelength Zones are AWS infrastructure deployments that embed AWS compute and storage services within telecommunications providers’ data centers
- Use the AWS Authenticator Configuration Map for cluster auth (`aws-auth-ConfigMap`)



