---
layout: post
title: AWS Certified Developer Associate
categories: [AWS]
tags: [AWS]
modified: 2021-02-04
---

This posts is about notes that I took during exam preparation.


#### Table of Contents

1. TOC
{: toc}


## Deployment

### Elastic Beanstalk

#### Environments

##### Web Server Environment

Single Instance Environment
* EC2 instance
* Security group
* S3 bucket
* Cloudwatch alarms
* Cloudformation stack
* Domain name
* ASG, desired=1
* No ELB
* Public IP address used

Load Balanced Environment
* ASG
* ELB
* Design to scale

##### Worker Environment

?

#### Deployment Policies

##### (1) All at once

Steps
1. Deploy new version of apps on all instances at same time.
2. Take all instances out of services during deploy.
3. Servers become available again after deploy.

Notes,
* Fast, but dangers
* In case of failure, re-deploy previous version

##### (2) Rolling

Steps
1. Deploy new version of apps on a batch of instances at a time
2. Take batch instances out of service during deploy
3. Re-attach updated instances
4. Got to next batch, and repeat 1-3

Notes,
* Not apply to single instance environment, as need load balancer
* In case of failure, take additional rolling update for rollback.



##### (3) Rolling with additional batch

Steps,
1. Launch new instances used to replace batch
2. Deploy the new batch
3. Attach new batch and terminate existing batch

Notes,
* Ensure capacity nenver reduced
* In case of failure, take additional rolling update for rollback
* Not apply to single instance environment, as need ELB

##### (4) Immutable

Steps,
1. Create a new ASG with new EC2 instances
2. Deploy new version of apps on new EC2 instances
3. Point ELB to new ASG, terminate old ASG with old EC2 instnaces.

Notes,
* Safest way to deploy critical apps.
* Terminate new EC2 instances, as existing instances are still running.


#### Deployment Methods

##### (1) All at once

* Fastest deploy time
* No DNS change
* Experience downtime
* In case of failure, manual rollback

##### (2) Rolling

* Single batch out of service
* No DNS change
* No downtime
* Deploy time longer than All ato once, vary
* In case of failure, manual rollback
* Successful batches running new version code

##### (3) Rolling with additional batch

* Minimal if first batch fails
* No DNS change
* No downtime
* Deploy time longer than rolling, vary
* In case of failure, manual rollback
* Successful batches running new version code

##### (4) Immutable

* Minimal impacts
* No downtime
* Deploytime longer than rolling with additional batch
* In case of failure, terminate new deploy
* Run new version code

##### (5) Blue/Green

* Minimal impacts
* No downtime
* Deploy time longer than rolling additional batches
* DNS changes
* In case of failure, swap URL
* Run new version code.


#### Deployment

##### (1) In-Place

* Within the scope of EB environment, and all deployment policies could be considered In-Place.
* Within the scope of same server, and all deployment policies do not involve server being replaced.
* Within the scope of uniterruputed server.

##### (2) Blue/Green

* Swapping EB environments
* Swapping out of ELBs
* Occurs at DNS level
* Requires databases are out side of environments


#### Configuration File

##### Where
* .ebextensions, 
* .config

##### What

* option settings
  * environment manifest - env.yml
  * environment name
  * solution stack, like Python, Ruby
  * associate environment links
  * default AWS service configurations
  * groups with symbol +
* Linux/Windows server configurations
  * Linux
    * packages
    * groups
    * users
    * files
    * commands
    * services
    * container commands
  * Windows
* custom resources

#### CLI

* eb init
* eb create
* eb status
* eb health
* eb logs
* eb events
* eb open
* eb deploy
* eb config
* eb terminate

#### Custom Images

* AWS Docs
* CLI
* describe-platform-version
* ImageId
* EC2
* Sessions Manager
* New AMI
* Update EB environment

#### RDS Configuration

##### (1) Inside of EB env

* for development
* RDS terminated with EB

##### (2) Outside of EB env

* for production
* RDS remained when EB terminates.

#### Single-container Dockerfile

?


## Development with AWS Services

### ECR

A fully-managed docker container registration service makes it easy for developers
to store, manage, and deploy docker container images.

Other serices that take images from ECR
* ECS
* Fargate
* EKS
* On-Promise


### ECS

Fully-managed container orchestration service. 
Highly secure, reliable and scalale way to run containers.

#### Components

* **Cluster**: Multiple EC2 instances which will house the docker containers.
* **Task Definition**: A JSON file that defines the configuration of containers (up to 10) wanting to run.
* **Task**: Launched containr run defined in Task Definition. Task does not remaining running once workload is complete.
* **Service**: Ensure tasks long running, eg. web-app
* **Container Agent**: Binary on each EC2 instance which monitors, starts and stops tasks.

#### Cluster

* Spot vs On-Demand
* EC2 instance type, eg. t3.large
* Number of instances
* EBS storage volume
* EC2 AMI type, eg. Amazon Linux 2 or Amazon Linux 1
* VPC, choose or create
* Container instance IAM role, eg. ecsInstanceRole
* CloudWatch container insights, optional
* Key Pair, ssh into EC2 instances


#### Task Definition

* It's a JSON file
* One task definition can define multiple containers.
* docker images can come from ECR or DockerHub.
* Must have one essential container, if essential container stops, then all other containers stop as well.


#### IAM Roles

* ecsInstanceRole, Role > EC2 > AmazonEC2ContainerServiceforEC2Role:
* ecsTaskExecutionRole, Role > Elastic Container Serice > Elastic Container Service Task > AmazonECSTaskExecutionRolePolicy


### Fargate

Serverless containers, do not need to worry about servers.

* Empty ECS cluster (no EC2 provision) can launch Fargate tasks.
* No longer have to provision, configure, and scale clusters of EC2 instances to run containers.
* Charge at least one minute, then by second.
* Pay based on duration and consumption.


#### Fargate Task Definition

* Define task definition meomory and vCPU.
* Add containers and allocate memory and vCPU for each.
* When run task, choose VPC and subnets.
* Apply a Security Group to a task.


#### Fargate vs Lambda

|             | Fargate             |  Lambda   |
| ------------| ------------------- | --------- |
| Cold Starts | Yes (shorter)       |   Yes     |
| Duration    | As long as you want | 15min (max) |
| Meomory     | Up to 30 GB | Up to 3 GB |
| Containers  | Provide your own containers | Limited to Standard containers | 
| Integration | More manual labor | Seamless integration with other serverless services |
| Pricing     | Pay at least 1 min, additional by seconds| Pay per 100ms |


### X-Ray

X-Ray is a distributed tracing and performance monitoring system, helps developers analysis and debug applications utilizing microservice architecture. It collects data about requests that application serves, views and filters data to identify issues or avenues for optimization.

What is Microservices?
* What is Microservice architecture? It's an architectural and organizational approach of software development by using small independent services to communicate.

What is Distributed Tracing?
* Distributed tracing (or, distributed request tracing) is a method used to profile and monitor applications, especially those built using microservice architecture.

What is Performance Monitoring?
* Application Performance Monitoring (APM) services help detect and diagnose complex application problems to maintain service at an expected performance level.

Overall, it helps pinpot where failtures occur, what causes poor performance.


#### Components

* AWS console
  * visualize, monitor and trace down request data, where the data was collected from X-RAY API.
* X-Ray API
  * communicate with AWS console.
* X-Ray Daemon: Buffer segments and send them in batches to X-Ray API.
* X-Ray SDK
  * interceptors to add to your code to trace incoming HTTPS requests
  * client handlers to instrument AWS SDK clients that your app uses to call other AWS services.
  * An HTTP client to use to instrument calls to other internal/external HTTP web services.
  * Instrumenting calls to SQL databases.


#### Instrumentation

What is instrumenting?

The ability to monitor or measure the level of a product's performance, to diagnose errors, and to write trace information.


#### X-Ray Daemon

X-Ray SDK sends JSON segment documents to a daemon process listening for UDP traffic, instead of sending trace data directly to X-Ray.

The X-Ray daemon buffers segments in queue and uploads them to X-Ray in batches. The daemon is available for,
* Linux
* Windows
* Mac
* Elastic Beanstalk
* Lambda platforms

X-Ray uses trace data to generate service graph.


#### Concepts

* segments
* subsegments
* traces
* service graph
* sampling
* trace header
* filter expressions
* groups
* annotations and metadata
* errors, faults, and exceptions

##### Service Graph

The graph can show:
* the client
* front-end
* back-end

The graph can tell:
* the bottlenecks
* the latency spikes
* the other issues

##### Segments

A segment can contain the following information:
* the host: hostname, alias, IP address
* the request: method, client address, path, user agent
* the response: status, content
* the work done: start and end times, subsegments
* issues that occur: errors, faults, and exceptions (automatic capature of exception stacks).

##### Subsegments

* granular timing information
* downstraming calls of original request
* Define arbitrary subsegments

##### Traces

* A trace collects all segments generated by a single request.
* A trace ID tracks the path of a request through your app.

Add a trace ID header to the request, and propagates it downstream to track the latency, disposition, and other request data.

##### Simpling

X-Ray uses a sampling algorithm to determine which request to trace.
By default, it records the first request each second, 5% of any additional requests.
This helps save money.

##### Trace Header

A percentage of reuqests are traced to avoid unnecessary costs, the trace header includes:
* sampling decision
* trace ID
* Or, a parent if request originated from an instrumented app.

##### Filter Expressions

Even with sampling, a complex app can still generate a lot of data. Filter expressions can help narrow down specific paths and users, and group results.


##### Groups

Create a group for assigning to filter expression, then call the group by name or ARN to generate its own service graph, trace summaries, and cloudwatch metrics.


##### Annotations and Metadata

Annotations and metadata can be added into segments/subsegments.

* Annotation key-value pairs are indexed for use with filter expressions. Up to 50 annotations per trace.
* Metadata key-value pairs that are not indexed. The values can be of any type, including objects and lists.


##### Erros, Faults, and Exceptions

* Error: client errors
* Fault: server faults
* Throttle: throttling errors


#### X-Ray Integration

##### Supported AWS Services

* Lambda
* ELB
* API Gateway
* SNS
* SQS
* EC2
* ECS
* ECS Fargate
* CloudWatch
* CloudTrail
* AWS Config
* App Mesh
* EB

##### Supported Languages

* Go
* NodeJS
* Java
* Python
* Ruby
* PHP
* ASP.NET


### ACM

Amazon Certificate Manager - provision, manage and deploy public/private SSL/TLS certificates for use with AWS services.

Two kinds of ACM
* public, free
* private, $400/month

ACM can handle multiple subdomains and wildcard domains, and can be attached to these services:
* ELB
* CloudFront
* API Gateway
* EB through ELB

Two pattern examples:
1. internet -> Route53 -> ALB (ACM Certificate) -> EC2 instances
2. internet -> Route53 -> ALB -> EC2 instances (Let's Encrypte)


### Route53

Highly available and scalable cloud Domain Name System (DNS).

* register and manage domains.
* create various record sets on a domain
* implement complex traffic flows/rules, eg. failovers, blue/green deploy.
* continuously monitor records via health checks
* resolve VPC's out of AWS.

#### An Use Case

Use Route53 to route traffic to custom domains with different AWS services.
* app.example.com -> ELB
* ami.example.com -> EC2
* api.example.com -> API Gateway
* www.example.com -> CloudFront
* minecraft.example.com -> Elastic IP, eg. 2.123.45.67

#### Record Sets

Record sets allows people to point naked domain and subdomains via domain records. For example, send www.example.com subdomain using a record to point to a Elatic IP.

#### Alias Record

* AWS has their own special Alias Record.
* Alias records can detect the IP change, and continously keep endpoint pointed to correct resources.

#### Traffic Flow

A visual editor for creating sophisticated routing configurations for your resources using existing routing types.

#### Routing Policies

7 different types of routing policies
1. Simple routing
2. Weighted routing
3. Latency-based routing
4. Failover routing
5. Geolocation routing
6. geo-proximity routing
7. multi-value answer routing

##### Simple Routing Policies

* Default policy
* Have 1 record, and multiple addresses
* when multiple value selected, then return one to user randomly.

##### Weighted Routing Policies

Use weighted values to split traffic.

For example, route 80% traffic to ELB a for most users, route 20% traffic to ELB b for user testing new features.


##### Latency-based Routing Policies

Route to region resources with low latency, it's based on region, eg. us-west-2, us-east-1.

For example, you have two copies of web-app backed by ALB, one in California, US, one in Montreal, Canada. A request comes from Denver will be route to California, another request comes from Toronto will be routed to Montreal for lower latency.

##### Failover Routing Policies

Route53 monitors the health-checks of primary sites and determine the health of endpoints, if primary endpoints is determined failure, then all traffic will be routed to secondary endpoint.


##### Geolocation Routing Policies

Detect traffic based on the geographic location of where requests come from, then route based on user's geolocations.

##### Geo-proximity Routing Policies

Direct traffic based on geographic locations of users and AWS resources, can adjust bias value.

##### Multi-value answer Routing Policies

* Respond to DNS queries with up to 8 healthy records selected at random.
* Similar to Simple Routing, but with an added health check for record set resources.


#### Health Checks

* Check health every 30s by default, and be reduced to 10s.
* Can initial a failover if unhealthy.
* A cloudwatch alarm can be created to alert unhealthy status.
* Can monitor other health checks to create a chain of reactions.


#### Resolver

A regional service that route queries between your VPCs and your network, it's a DNS resolution for hybrid environments (On-Premise and Cloud).


### KMS

Key Mangement Service can create, control and rotate your encryption keys used for encrypting your data on AWS.

#### KMS

* Multi-tenant Hardware Security Module (HSM)
  * Most AWS services can just checkbox on Encryption
  * and then choose a KMS key
* Can be used with CloudTrail to audit access history.
* Has been integrated with many AWS services, EMR, RDS, Athena, X-Ray, CodeBuild ...

#### HSM
* hardware that is specified for storing your enctyption keys.
* designed to be tamper-proof
* store keys in-memory, never written to disk.

#### Multi-tenant

* Multiple customers are utilizing the same piece of hardware.
* Customers are isolated from each other virtually.
* If one user use entier piece, that called single-tenant.
* CloudHSM is a single-tenant HSM, dedicated HSM means stricter compliance. Level-3
* KMS is only FIPS 1402 Level 2.

#### CMKs

Customer Master Keys (CMKs)

*What is encryption?*

The process of encoding a message or information in such a way that only authorized parties access it and those who are not authorized cannot.

*What are cryptographic keys (data key)?*

A string of data that is used to lock or unlock cryptographic functions, including authentication, authorization, and encryption.

*What is a Master Key?*

Stored in a secure hardware. Master keys are used to encrypte all other keys on a system.

*What is Envelope Encryption?*

A key used to encryption another key.

Master Key -> Data Key -> Data

*So, what's CKM?*

CKMs are the primary resources on KMS, a CMK is a logical representation of a Master key.

CMK metadata:
* key id
* creation date
* description
* key state

CMK contains key material to:
* encrypte data
* decrypte data

AWS KMS supports:
* symmetric CMKs, 256-bit key, One key. eg. encrypting a S3 bucket using AES-256.
* asymmetric CMKs, RSA key, Two keys, eg. EC2 key pairs used to ssh into a server.

#### KMS CLI

* aws kms create-key
* aws kms encrypt
* aws kms decrypt
* aws kms re-encrypt
* aws kms enable-key-rotation


### Cognito

What Cognito can do?

* Decentralized managed authentication. 
* Sign-up, sign-in integration for your apps.
* Social identity provider, eg. Google, facebook

#### Concepts

* Cognito User Pools: user directory with authentication to IpD to grant access to your app.
* Cognito Identity Pools: Provide temporary credentials for users to access AWS services.
* Cognito Sync: Syncs user data and preferences across all devices.

#### Web Identity Federation

To exchange identity and security information between an IdP and an application.

##### Identity Provider (IdP)

A trusted provider of your user identity that lets you use authenticate to access other services.

* Facebook
* Amazon
* Google
* Twitter
* GitHub
* LinkedIn

And,

* OIDC - OpenID Connect, a type of IdP which uses Oauth
* OAuth
* OpenID

##### Types of IdPs

Technologies behind the IdPs

* SAML - Security Assertion Markup Language, a type of IdP which is used for SSO.
* SSO - Single Sign On

#### Cognito User Pools

User pools are user directories used to manage:

* Sign-up
* Sign-in
* Account recovery
* Account confirmation

With these actions for your web and mobile apps, so that:

* Allow users to sign-in directly to the User Pool, or using Web Identify Federation
* Use AWS cognito as the identity broker between AWS and the IdPs.
* Successful user authentication generates a JSON Web Token (JWT)
* User Pools can be thought of as the account used to access the system.

Operations when create a User Pool:

* Choose the attributes
* Choose password requirements
* Apply MFA
* Retrict whether users are allowed to signup on their own, or need admin verification
* Aanalytics with PinPoint for user campaigns
* Trigger custom log via Lambdas after actions, eg. signup


#### Indentity Pools

Identity Pools provide temporary AWS credentials to access AWS services, eg. S3, DynamoDB. It can be thought of as the actual mechanism authorizing access to AWS resources.


#### Cognito Sync

* Sync user data and preferences across devices with online of code.
* Cognito uses *push synchronization* to push updates and synchronize data.
* Use SNS to send notifications to all user devices when data in the cloud changes.


### SNS

Simple Notification Service, subscribe and send notifications via text message email, webhooks,
lambda, SQS, and mobile notifications.

*What is Pub/Sub?*

A publish-subscribe pattern implemented in messaging systems. Publishers -> Event Bus -> Subscribers.

* Publisher don't know who their subscribers are.
* Subscribers does not pull for messages
* Messages are automatically pushed to subscribers.
* Messages and events are interchangable terms in pub/sub.

SNS is a highly available, durable, secure, fully managed pub/sub messaging service used to 
decouple microservices, distributed systems, and serverless apps.

* Publishers push messages to a SNS Topic.
* Subscribers subscripe to SNS Topic to have messages pushed to them.


#### SNS Topics

* Allow you to group multiple subscriptions together.
* A topic is able to deliver to multiple protocols at once, eg. email, text message, http(https)
* Publishers do not care about the subscribers protocol.
* Topics can automatically format messages into subscribers' chosen protocol.
* Can encrypt Topics via KMS.

#### SNS Subscriptions

A subscription can only subscribe to one protocol and one topic.

Protocols:
* Http
* Https
* Email
* Email-JSON
* SQS
* Lambda
* Platform Application Endpoint
* SMS

##### Application as Subscriber

Send push notification messages directly to apps on mobile devices.


### SQS

Simple Queue Service, Fully managed queuing service used to decouple and scale microservices,
distributed systems, and serverless applications.

*What is messaging systems?*

Used to provide asynchronous communication and decouple processes via messages/events from sender to receiver, or publisher to consumer.


SQS is for Application Integration, a queue is a temporary repository for messages that are wating for processing.


#### Queuing vs Streaming

* Queuing
  * Delete messages once consumed
  * Simple communication
  * Not real-time, have to pull
  * Not reactive
* Streaming
  * Mutiple consumers can react to messsages/events.
  * Event live in stream for long periods of time, so complex operations can be applied.
  * Real-time

#### SQS Limits


##### Message Site

* Message size, 1 byte -> 256 KB
* Amazon SQS Extended Client Libarary for Java, 256KB -> 2G

##### Message Retention

* by default, 4 days
* minimum 60 seconds
* maximum 14 days

#### Queue Types

##### Standard Queue

* Allow nearly-unlimited number of transactions per second.
* Gurantee that a message will be delivered AT LEAST once.
* More than one copy of a message could be potentially delivered out of order.
* Provide best-effort ordering to ensure a message is generally delivered in the same order that it was sent.

##### FIFO Queue

* First-in-First-Out queues
* Support multiple ordered message groups in one queue.
* Limited to 300 transactions per second
* SWS FIFO queues have same capabilities of a Standard Queue.


#### Visibility Timeout

After a consumer picked up a message, then the message would be insible in the SQS queue to other
consumers. This period of time is called visiblity time-out.

* The consumer will delete the message from queue after it got processed.
* If the consumer does not finish the process before the visiblity time-out, then the message will be visible again in the queue.
* So, same message could be delivered multiple times.

Time-out Values
* default: 30 seconds
* minimum: 0 seconds
* maximum: 12 hours

#### Short vs Long Polling

* Short polling (default)
  * Return messages immediately, even if the message queue is empty
  * When you need a message right away.
* Long polling
  * Waits until message arrives in the queue
  * or long poll timeout expires.
  * Inexpensive to retrieve messages
  * Reduce the number of empty receives.
  * Most use cases.


### Kinesis

Scalable and durable real-time data streaming service for data ingestion and analysis in real-time
from multiple sources.

4 types of Kinesis Streams:
* Data Streams
* Firehose Delivery Streams
* Data Analytics
* Video Analytics

#### Data Streams
* Pay per running shard
* Have multiple consumers, must configure consumers manually. eg.
  * Redshift
  * DynamoDB
  * S3
  * EMR
* Data persists from 24 hours (default) to 168 hours.

#### Firehose Delivery Streams
* Choose one consumer from a predefined list
* Data immediately disappears once it's consumed
* Can convert incoming data to other formats, compress and secure data.
* Pay only for data that is ingested.

#### Video Streams
* Ingest video and audio encoded data from various devices or services.
* Output video data to ML or video processing services.
  * SageMaker
  * Rekognition
  * Tensorflow
  * Custom process

#### Data Analytics
* Specify Firehose or Data Streams as an input and an output
* Data that pass through Data Analytics is run through custom SQL provided


### SSM

SSM Parameter Store, a secure, hierarchical storage for configuration data and secrets management.

* hiearchies
* track versions
* can encrypt
* using forwards slashes

Type:
* String
* StringList
* SecureString

Tiers:
* Standard
* Advanced

#### Parameter Policies
* Forcing you to update or delete passwords
* Asynchronous, periodic scans ensure you don't need to perform additional actions to enforce the policy.
* Assign multiple policies

Types:
* Expiration
* ExpirationNotification
* NoChangeNotification 

#### CLI Hierarchy

* `aws ssm put-parameter`
* `aws ssm get-parameter-by-path`

### Secrets Manager

Protect secrets needed to access your applications and services. Mostly used to store and automatically rotate database credentials.

* $0.40 per secret per month
* $0.05 per 10,000 API calls
* CloudTrail can monitor credentials access

#### Automatic Rotation

For any database credentials


#### CLI Commands

* `aws secretsmanager describe-secret`
* `aws secretsmanager get-secret-value`


### DynamoDB

A key-value and document NoSQL DB which can gurantee consistent reads/writes at any scale.

* Key-Value Store
* Document Store

Features:
* Fully managed
* Multi-region
* Multi-master
* Durable database
* Built-in security
* Backup and restore
* In-memory caching

#### Anatomy of DynamoDB
* Tables
* Items
* Attributes
* Keys
* Values

#### Read Consistency
For each db, there are 3 copies in different AZs. When data needs to be updated, it has to write updates to all copies. It is possible for data to be inconsitent when reading from a copy which has not been updated.

##### Eventual Consistent Reads (default)
* Reads are fast
* No gurantee of consistent
* Become generally consitent within a second

##### Strongly Consistent Reads
* No result return util all copies are consistent
* Guranteed consistency
* Higher latency, slower reads
* Will be consistent within a second

#### Partitions

A partition is an allocation of storage for a table, backed by SSDs and automatically replicated across multiple AZs within an AWS region.

* starts off with a single partition
* automatically creates partitions as your data grows
  * for every 10GB data
  * when exceed the RCUs or WCUs for a single partition.
* Each partition has maximum of 3000 RCUs and 1000 WCUs.
  * Read Capacity Units
  * Write Capacity Units

#### Primary Keys

* Defined when create a table
* Determines where and how data being stored in partitions
* Can not be changed later

##### Simple Primary Key

How a Primary Key with only a Partition Key chooses which partition.
* Partiton Key has to be unique
* Secret algorithm internally decides which partition to write data.

##### Composite Primary Key
How a Primary key with a Partition Key and Sort Key chooses which partition.
* Combination of partition and sort key have to be unique
* Secret algorithm internally decides which patition to go.

##### Primary Key Design
* For simple primary keys, no two items can have same partition key.
* For composite primary keys, two items can have same patition key, but partition and sort key combined must be unique.

Two things when design
* Distict - should be unique as possible
* Uniform - should evenly devide data

#### Query and Scan

##### Query

* Find items in table based on primary key values.
* Eventually consistent reads by defaults
* ConsistentRead=True to enable Strongly consistent
* Return all attributes for items by default
* Use ProjectExpression to filter specific attributes.
* Sorted ascending by default, use ScanIndexForward to descending.

##### Scan
* Scan through all items and then return one or more iterms through filters.
* Return all attributes for items
* Use ProjectExpress to filter specific attributes
* Scan operations are sequential, can speed up by using Segments and Total Segments

Avoid scan when possible.
* Much less efficient than query
* Large table takes longer to complete
* Can use all your provisioned throughput in a single scan

#### Provisioned Capacity

Maximum amount of capacity your application is allowed to read or write per second from a table or index.

* RCUs
* WCUs

If go beyond capacity you'll get the error ProvisionedThroughputExceededException, aka, Throttling.

#### On-Demand Capacity

Pay per request, it's good for:
* New tables with unknown workloads
* Unpredictable application traffic
* Paying for only what you use

Limited by default upper limits for a table,
* 40,000 RCUs.
* 40,000 WCUs.

#### Caculating Reads

##### Strong Consistent Rreads

Steps
1. Round data up to nearest 4
2. Divide data by 4
3. Times by number of reads

##### Eventual Consitent Reads

Steps
1. Round data up to nearest 4
2. Divide data by 4
3. Times by number of reads
4. Divide final number by 2
5. Round up to the nearest whole number


#### Calculating Writes

Steps
1. Round data to nearest 1
2. Times by number of writes

#### Global Tables

Deploy a multi-region, multi-master database, must meet
1. User KMS CMK
2. Enable Streams
3. Stream Type of New and Old Image

#### Transactions

*What is transaction?* Represents a change that will occur to the database. If any dependent conditions fail, then a transaction will rollback as if the database changes never occurred.

* All-or-nothing changes to multiple items within and across tables
* Performs 2 underlying reads/writes of every item in the transaction.

#### Time To Live (TTL)

TTL lets itmes in DynamoDB expire (deleted) at a given time. It's great for keeping db small, and suited for temp data.

#### Streams
Enable a stream on a table, DynamoDB captures every modification to data items so you can react to the changes.

eg. when insert, update, or delete items, send event to lambda.

#### Errors

* ThrottlingException
* ProvisionedThroughputExceededException

#### Indexes

A database index is a copy of selected columns of data in a database which is used to quickly sort.

* Local Secondary Index
  * created with initial table
  * can provide strong consistency
* Global Secondary Index
  * should generally use
  * can not provide strong consistency

##### Local Secondary Index
* local in every partition
* partition has the same partition key value
* total size of indexed items for any one partition key value < 10GB
* shares provisioned throughput.
* limit to 5 per table (default)

##### Global Secondary Index
* global, due to queries on the index can span all of the data in the base table, across all partitions.
* indexes has no size restriction.
* have their own provisioned throughput settings, not consume from base table.
* limit to 20 per table (default)

##### LSI vs GSI

* Key schema
* Key attributes
* Size restriction per partition key value
* online index operations
* queries and partitions
* read consistency
* provisioned throughput consumption
* projected attributes

#### Accelerator(DAX)
In memory cache for DynamoDB that runs in a cluster, which can reduce response times to microseconds!

* A DAX cluster consists of one or more nodes
* Each node run its own instance of DAX caching software.
* One node serve as primary node
* Additional nodes serve as read replicas
* Your app can access DAX by specifying the endpoint for the DAX cluster.
* DAX software performs intelligent load balancing and routing.
* Incoming requests are evenly distributed across all of the nodes.

Idea for:
* fastest possible response time
* read small number of items
* read-intensive, but also cost-sensitive
* require repeated reads against a large set of data.

Not ideal for:
* strongly consitent reads
* do not require microsecond reponse times for reads
* write-intensive, or that do no perform much read activities
* already using different caching solutions.

Ultimate DynamoDB Cheatsheet!!! 5:50:00


### EC2

Choose OS, storage, memory, network throughput, and lauch and ssh into server within minutes.

#### Instance Types

* General Purpose - A1, T3, T3a, T2, M5, M5a, M4
* Compute Optimized - C5, C5n, C4
* Memory Optimized - R5, R5a, X1e, X1, Z1d, High Memory
* Accelerated Optimized - P3, P2, G3, F1
* Storage Optimized - I3, I3en, D2, H1

#### Instance Profile

Attach a role to an instance via an Instance Profile so that your instance has permissions to
access certain services.

IAM Policy -> IAM Role -> Instance Profile -> Ec2 Instance

#### Placement Groups

Let you to choose the logical placement of your instances to optimize for communication, performance,
or durability.
* Cluster
* Partition
* Spread

#### Userdata

UserData is a script that will be automatically run when lauching an EC2 instance, installing 
packages, update softwares, etc.

#### Metadata

curl http://169.254.168.254/latest/meta-data


### VPC

Core Components

1. Account
2. Region
3. VPC
4. Availability Zone
5. Subnet (public, private)
6. Security Group
7. NACL
8. Route Table
9. Router
10. Internet Gateway
11. Internet

Key Features

* Region specific, do not span regions
* Upto 5 VPC per region
* Every region comes with a default VPC
* 200 subnets per VPC
* IPv4/IPv6 cidr blocks
* Cost nothing: VPC, route table, internet gateway, security group, subnets, VPC peering
* Cost: NAT gateway, VPC endpoints, VPN gateway, Customer gateway
* DNS hostnames

#### Default VPC

* with a size with 16 IPV4 cidr blocks
* a size with 20 default subnets in each AZ.
* an internet gateway
* default security group
* default network access control list (NACL)
* associate default DHCP options
* a main route table

#### Default Everywhere IP

0.0.0.0/0, all possible IP addresses.

#### VPC Peering

Allow you to connect one VPC with another over direct network route using private IP addresses.

* behave like same network
* across same or different accounts/regions.
* start configuration, 1 central, 4 others
* no transitive peering
* no overlapping cidr blocks.

#### Route Table

* To determine where network traffic is directed.
* Each subnet in VPC must be associated with a route table
* Each subnet can only associate to one route table at a time.


#### Internet Gateway (IGW)

Allow VPC access to the internet, IGW does two things:
* Provide a target in your VPC route tables for internet-routable traffic.
* Perform network address translation (NAT) for instances that have been assign public IPv4 addresses.


#### Bastions / Jumpbox

Bastions (Known as Jumpbox) are EC2 instances which are security harden, designed to help gain access to
EC2 instances via SSH or RCP that are in private subnet.

NATs cannot/should not be used as Bastions.

System manager's session manager replaces the need for Bastions.

#### Direct Connect

Dedicated network connections from on-primises locations to AWS.

* very fast netowrk
* reduce network costs and increase bandwidth troughput
* more consistent network experience

#### Auto Scaling Groups (ASG)

It contains a collection of EC2 instances that are treated as a group for the purposes
of automatic scaling and management.

Auto scaling can occur via:
1. Capacity Settings
2. Health Check Replacements
3. Scaling Policies

#### Capacity Settings

* Min
* Max
* Desired Capacity

#### Health Check Replacements

* EC2 health check type: 
ASG performs a health check on EC2 instances to determine if a software/hardware issue
based on EC2 Status Checks.

* ELB health check type: 
ASG performs a health check based on ELB health check (https ping).

#### Scaling Policies

* Target tracking scaling policy, eg. CPU utilization exceeds 75%
* Simple scaling policy (lagcy, not recommended)
* Scaling policies with steps.

#### ELB Integration

ASG can be associated with ELB.

* Classic load balancer (CLB) are associated directly to the ASG.
* ELB/NLB are associated indirectly via their Target Groups.

#### Launch Configuration

It's an instance configuration template that an ASG uses to launch EC2 instances.

It cannot be edited, when need to update, you need to create a new one, or clone.


#### VPC Endpoints

VPC endpoints allow you to privately connect to your VPC to other AWS services, and
VPC endpoint services.

Two types of VPC endpoints
* Interface endpoints
* Gateway endpoints

##### Interfact Endpoints

Cost Money.

Elastic Network Interface (ENI) with a private IP address, powered by AWS PrivateLink

Supports many services.


##### Gateway Endpoints

Free.

It's a gateway that is a target for a specific route in your route table, used for
traffic destined for a supported AWS service.

It currently supports 2 services:
* S3
* DynamoDB

#### ELB

Distributes incoming application traffic across multiple targets, e.g
* EC2 instances
* containers
* IP addresses
* Lambda functions

Three types of ELB:
* ALB
* NLB
* CLB


##### ELB Rules of Traffic

* Listeners
* Rules (not for CLB)
* Target Groups (not for CLB)

##### ALB

Designed to balance HTTP/HTTPS traffic

* OSI layers, at Layer 7 (Application)
* Request routing allows to add routing rules to your listener based on HTTP protocol
* Web application firewall (WAF) can be attached to ALB
* Great for web apps.

##### NLB

Designed to balance TCP/UDP

* OSI layers, Layer 4, Transport
* Handle millions of requests per second, extremely low latency
* Cross-zone load balancing
* Great for multi-player video games.

##### CLB

Legacy.

* Can balance HTTP, HTTPS, TCP traffic (not same time)

##### Sticky Sessions

It an advanced load balancing method that allows you to bind a user's session to a specific EC2 instance.

* Ensure requests from that session are sent to the same instance.
* typically utilized with a CLB
* can be enabled for ALB through Target Group, not EC2 instances.
* Cookies are used to remember which EC2 instances.

##### X-Forwarded-For (XFF) Header

XFF header is a command method used for identifying the originating IP address through
HTTP proxy or a ELB.


##### ELB Health Checks

Instances are monitored by ELB, report back health checks as InService, OutofService.


##### Cross-Zone Load Balancing

Only for CLB, NLB

##### Request Routing

Apply rule to incoming request and then forward or redirect traffic.

* Host header
* Http header
* Source IP
* Http header method
* Path
* Query string


#### Security Groups

A virtual firewall that controls the traffic to and from EC2 instances.


##### Take Aways

* sg are associated with EC2 instances.
* sg are at protocol and port access level.
* sg are a set of rules filtering inbound/outbound traffic through EC2 instances.
* All traffic is blocked by default unless a rule specifically allows it.
* multiple instances across multiple subnets can belong to a sg.

##### Limits

* upto 10,000 in a region, default 2,500
* 60 inbound rules, 60 outbound rules per sg
* 16 sg per ENI, default 5


#### Network Access Control List (NACL)

A layer of security that acts as a firewall for controlling traffic in/out of subnets.


##### Take Aways

* Acts as firewall at the subnet level
* VPCs automaticall get a default NACL
* Subnets are associated with NACLs, and can only belong to a single NACL.
* Each NACL contains rules that deny/allow traffic into/out of subnets.
* Rule # determines the order of evaluation.
* Can block a single IP address, can not do with sg.


### IAM

Manage access of AWS users and resources.


#### Core Compoents

1. users
2. groups
3. roles
4. policies


#### Type of Policies

1. Managed Policies, orange box
2. Customer Managed Policies, no symbol
3. Inline Policies


#### Access Keys

* programmatically vis CLI or SDK

#### MFA

* Turned on per user
* User needs to turn on

#### Temporary Security Credentials

Like access keys, but temporary, useful for
* identity federation
* delegation
* cross-account access
* IAM roles

##### Identify Federation

Linking a person's electronic identity and attributes, stored across multiple
distinct identity management systems.

* Enterprise identity federation
  * SAML
  * Custom Federation broker
* Web identity Federation
  * Amazon
  * Facebook
  * Google
  * OpenID Connect (OICD) 2.0


##### Security Token Service (STS)

A global service enables you to request temporary, limited-privilege credentials
for Iam users or federated users.

STS Returns:
* Access Key ID
* Secret Access Key
* Session Token
* Expiration

The following API actions to obtain STS:
* AssumeRole *
* AssumeRoleWithSAML
* AssumeRoleWithWebIdentity *
* DecodeAuthorizationMessage
* GetAccessKeyInfo
* GetCallerIdentity
* GetFederationToken
* GetSessionToken


#### Policy Structure

* version
* statement
* sid (optional)
* effect
* principal
* action
* resource
* condition


### CloudFront

Content Distribution Network (CDN)

Creates cached copies of your website at various Edge locations around the world.

CDN - a distributed network of servers, deliver web page/content based on
* geographical location
* origin of the web page
* content delivery server



#### Core Components

* origin
  * S3 bucket
  * EC2 instance
  * ELB
  * Route53
* edge location
  * different AWS region or AZ

#### Distributions

Two types of ditributions
1. Web (for website)
2. STRM (for streaming media)

* Behaviors
* Invalidations
* Error Pages
* Restrictions

#### Lambda@Edge

Four functions
1. Viewer request - when cloudfront receives a request from a viewer.
2. Origin request - before cloudfront forwards a request to the origin.
3. Origin response - when cloudfront receives a response from the origin.
4. View response - before cloudfront returns the response to the viewer.

#### Protection

Original Identiry Access (OAI)

* Signed URLs
* Signed Cookies

In order to use Signed URLs or Signed Cookies, you need to have an OAI.


### CloudTrail

Logs API calls between AWS services when you need to know who to blame.

AWS CloudTrail is a service that enable sgoverance, compliance, operational auditing,
and rist auditing of your AWS account.

Used to monitor API calls and Actions on an AWS account, and can easily identify which users and accounts made the call to AWS eg.

* Where
* When
* Who
* What

#### Event History

* Last 90 days via Event history by default

#### Trail Operations

* log to all regions
* across all accounts in an organization
* encrypt your logs
* log file validation

#### To CloudWatch

CloudTrail can be set to deliver events to a cloudwatch log.

#### Management Events vs Data Events

##### Management Events

* Configuring security
* Registering devices
* Configuring rules for routing data
* Setting up logging

##### Data Events

* Turned off by defaults


### CloudFormation

Infrastructure as Code (IaC)

#### Formats

* JSON
* YAML

#### Template Anatomy

* MetaData
* Description
* Parameters
* Mappings
* Conditions
* Transform
* Resources
* Outputs

#### Quick Starts

A collection of Pre-built cloudformation templates


#### Stack Updates

Two ways to perform update:
* Direct update
* Change sets

#### Prevent Stack Updates

To prevent data loss or interruption to service

* StackPolicy

#### Nested Stacks

1. Create modular templates, reuse
2. assemble large template, reduce comlexity

#### Drift Detection

* What is Drift
* Why does Drift happen
* Detect Drift
* Nest stacks & Drift detection

After perform drift detection, it could show:
* DELTED
* MODIFIED
* NOT_CHECKED
* IN_SYNC

#### Rollbacks

When create, update or destroy a stack, could encounter error.

#### Pseudo Parameters

parameters that are predefined by AWS cloudformation.

* AWS::Partition
* AWS::Region
* AWS::StackId
* AWS::StackName
* AWS::URLSuffix

#### Resource Attributes

* Creation Policy
* Deletion Policy
* Update Policy
* UpdateDelatePolicy
* Depends On

#### Instrinsic Functions

Use instinsic functions in your templates to assign values to properties that are
not available util runtime.

##### Ref

Ref returns different things for different resources

##### GetAttr

Allows to access many different variables on a resouce

#### Wait Conditions

Wait for a condition

* To coordinate stack resouce creation with configuration actions that are external to the stack creation
* To track the status of a configuration process

WaitCondition is very similar to CreationPolicy. AWS recommends to using CreationPolicy for EC2 and ASG.

* CreationPolicy waits on the dependent resouce
* WaitCondition waits on the wait condition (external).

### CDK

Write IaC using an imperative paradigm with favourite language.

What is transpiler?

Imperative infrastructure -> Declarative Infrastructure

### SAM

An extension of CloudFormation that let's you define serverless applications.

SAM is both an AWS CLI tool and a CloudFormation Macro which makes it effortless to define and deploy serverless applications.

CloudFormation allows to specify macros through the `Transform` attribute.

```json
Transform: 'AWS::Serverless-2016-10-31'
```

SAM gives new Resource types:

* `AWS::Serverless::Function`
* `AWS::Serverless::API`
* `AWS::Serverless::SimpleTable`

#### SAM vs CloudFormation

* Write at least 50% less code with SAM

#### SAM CLI

* `sam build`
* `sam deploy`
* `sam init`
* `sam local generate-event`
* `sam local invoke`
* `sam local start-api`
* `sam local start-lambda`
* `sam logs`
* `sam package`
* `sam publish`
* `sam validate`


### CI/CD

Code -> Build -> Integrate -> Test -> Release -> Deploy

* Continous Integration: from Code to Test
* Continous Delivery: from Code to Release
* Continous Deployment: from Code to Deploy 

#### CodeCommit

Key features:

* In scope with many compliance programs, eg. HIPPA
* Repositories are encrypted
* Can handle repo with large numbers of files or branches
* No limit on the size of repos or file types
* Close to your other prod resources
* Use IAM to control AWS user access to repos.

#### Docker

#### CodeBuild

A fully managed build servie in the cloud.
* Run unit tests
* Produce artifacts

##### Workflow

CodeBuild can be triggered by:
* AWS Console
* AWS CLI
* AWS SDK
* AWS CodePipline

Then, -> CodeBuild -> BuildEnvironment -> Build Artifact/SNS Topic

##### Build Environments

Different build environemtns

##### Buildspec.yml

It provides the build instructions, needs to be at the root of your project.

spec 0.1 - runs each build command in a separate instance
spec 0.2 - run all build commands in the same instance, recommended.

phases:
* install
* pre_build
* build
* post_build

artifacts, the build ouput


#### CodeDeploy

A fully-managed deploy pipeline to deploy to staging or prodution environments.

Can deploy:
* EC2
* On-Premise
* Lambda
* ECS

##### Core Components

1. Application
2. Deployment Groups
3. Deployment
4. Deployment Configuration
5. AppSpec File
6. Revision

##### In-Place Deployment

* The app on each instance in the deployment is stopped
* The latest app revision is installed, and the new version of app is started and validated.
* You can use load balancer so that each instance is deregistered during its deployment, the restored to service after the deployment is complete.
* Only deployments that use the EC2/On-Premises compute platform can use in-place deployment.

##### Blue/Green Deployments

* Instances are provisioned for the replacement environment
* The latest app revision is installed on the replacement instances.
* An optional wait time occurs for actitivies such as app testing and system verification.
* Instances in the replacement environment are registered with an ELB, causing traffic to be rerouted to them.
* Instances in the original environment are deregistered and can be terminated or kept running for other users.

##### Appspec.yml

##### Lifecycle Event Hooks

##### CodeDeploy Agent and Service Role

#### CodePipeline

A fully-managed CI/CD pipeline to setup automatic deployments.

Source -> Build -> Deploy

* Pipeline
* Stage
* Action Group
* Action
* Artifact
* Stage Trasitions

#### CodeStar

Quickly develop, build and deploy apps on AWS.

* Deployment pipeline
* Access management
* Project dashboard to monitor infrastructure

### RDS

A managed relational database service. support multiple SQL engines, easy to scale, backup and secure.

6 relational database:
1. Amazon Aurora
2. MySQL
3. MariaDB
4. PostgreSQL
5. Oracle
6. Microsoft SQL Server

#### RDS Encryption

You can turn on encryption at-rest for all RDS engines.

You may not be able to turn encryption on for older engines.

#### Backup

1. Automated Backups

  * Choose a Retention period between 1-35 days
  * Stores transaction logs throughout the day
  * Automated backups are enabled by default
  * All data is stored inside S3
  * There is no additional charge for backup storage
  * You defined your backup window
  * Storage I/O maybe suspended during backup

2. Manual Snapshots
  * Taken manually by the user
  * Backup persist even if you delete the original RDS instance.

#### Restoring Backups

When recovering AWS will take the most recent daily backup, and apply transaction log data relevant to that day. This allows point-in-time recovery down to a second inside the retention period.

* Backup data is never restored overtop of an existing instance.
* A new instance is created for the restored database.
* Restored RDS instance will have a new DNS endpoint.


#### Multi AZ

Ensure database remains available if another AZ becomes unavailable.

Summary:
* Synchronous replication - highly durable
* Only database engine on primary instance is active
* Automated backups are taken from standby
* Always span 2 AZ within a single region.
* Database engine version upgrades happen on primary
* Automatic failover to standby when a problem is detected.

#### Read Replicas

Read-Replicas allow you to run multiple copies of your database, these copies only allows reads and is intended to alleviate the workload of your primary database to improve performance.

* You must have automatic backups enabled to use Read Replicas
* Asynchronous replication happens between the primary RDS instance and the replicas.
* You can have up to 5 replicas of a database.
* Each read replica will have its own DNS endpoint.

Replica Types:
* Muti-AZ replicas
* Cross-region replicas
* Replicas of other read replicas

Summary:
* Asynchronous replication - highly scalable
* All read replicas are accessible and can be used for read scaling
* No backups configured by default
* Can be within an AZ, cross-AZ, cross-region
* Database engine version upgrade is independent from source instance.
* Can be manually promoted to a standalone database instance.

### S3

Object-based storage service.

What is Object Storage?

Data storage architecture that manages data as objects, as opposed to other storage architectures:
* file systems, manages data as files and file hierarchy.
* block storage, manage data as blocks within sectors and tracks.

S3 provides with unlimited storage, store data from 0 bytes to 5 TB in size of object.


#### Storage Classes

Consider:
* Retrieval Time
* Accessibility
* Durability

Classes:
* Standard (default)
* Intelligent Tiering
* Standard Infreently Accessed
* One Zone IA
* Glacier
* Glacier Deep Archive


#### S3 Security

* All new buckets are PRIVATE by default
* Logging per request can be turned on a bucket
* Access control - Bucket Policies and Access Control List (ACL)

#### S3 Encryption

* Encryption in transit
* Server Side Encryption (SSE)

#### Data Consistency

* Read after write consistency
* Eventual consistency

#### Cross-region replications (CRR)

When abled, any object that is uploaded will be automatically replicated to another regions.

You must have versioning turned on both the source and destination buckets.

Can happen across accounts.

#### S3 versioning

Version ID

Could not turn off, once enabled.

#### S3 Lifecycle Management

Automate the process of moving objects to different classes.

#### Transfer Acceleration

#### Presigned URLs

To provide temporary url to access private objects.

#### MFA Delete

Ensure users can not delete objects from a bucket unless the provide their MFA Code.

* AWS CLI must be used to turn on MFA.
* The bucket must have versioning turned on.


### CLI & SDK

#### CLI

Command Line Interface

* What actions CLI can perform?
* Important flags need to know.

#### SDK

Software Development Kit

Supported languages
* C++
* Go
* Java
* Javascript
* .NET
* NodeJs
* PHP
* Python
* Ruby

#### Access Key and Secrets

* programmatic access
* `~/.aws/credentials`
* multiple profiles


## Security



## Refactoring



## Monitoring & Troubleshooting



#### References
[1] https://www.youtube.com/watch?v=RrKRN9zRBWs&ab_channel=freeCodeCamp.org
