

# Misc

- [Misc](#misc)
  - [Life cycle configuration](#life-cycle-configuration)
  - [Replication](#replication)
  - [Pre-Signed URLS](#pre-signed-urls)
  - [S3 select and select glacier](#s3-select-and-select-glacier)
  - [S3 events](#s3-events)
  - [S3 Access Logs](#s3-access-logs)
  - [S3 object lock](#s3-object-lock)
  - [S3 access points](#s3-access-points)

## Life cycle configuration

- Set of rules 
- Rules consist of actions
- Can be run on a bucket or group of objects
- Transition actions - used to move objects to different storage classess based on the access - save costs
- Expiration actions - used to expire / delete based on time - tidy up
- Be aware of the smaller sizes transition as they can cost more in the lower classes
- S3 standard to move to infrequent access, we need to atleast wait for 30 days while transitioning from life cycle

## Replication

- 2 types
- Cross region replication - allows replication of source bucket into buckets in different region
- Same region replication
- Both support same account/different account
- Replication configuratipon spec
- Role does the replication based on the config
- In same account, iam role automically access to the both accounts, however for different accounts, destination need to add a bucket policy to allow source role to replicate objects.
- Replication config
  - All objects or some subset
  - Which storage class - default will be the same
  - Ownership - default is the source account
  - RTC - Replication time control - 15 min guarntee SLA
- By default - replication is not retro active, only from that point when it is enabled then only new objects will be replicated
- For replication, versioning need to be turned on
- One way replication
- Use batch replication to replicate older objects
- Replication can handle all encrypted type of objects
- Soruce bucket owner needs permssions to objets for replication to happen
- No system events (life cycle events), no glacier or glacier deep archive are replicated
- No deletes are replicated ( this can be added by delete marker replication)
- Why use replication ?
  - SRR - Log Aggregation
  - SRR - Prod and test sync
  - SRR - Resilience with strict soverignty
  - CRR - Latency reduction
  - CRR - Global Resilience improvements
- Demo
  - Create 2 buckets in different regions
  - enable static site hosting and versioning on both bucketes
  - create a replication rule for source to destination bucket
  - upload to source and check
  - delete the buckets and role created

## Pre-Signed URLS

- Way of giving access to object to other persons in a safe and secured way
- S3 bucket - private access, only people with the acess can acces it
- Non authenicated people cannot access it
- Without making public, we need to provide an account/creds to the user to access private account
- Pre signed urls comes to the rescue
- provide expiry/acess method/bucket-name/object-name and s3 gives a pre signed url
- non authenticated user can use this pre-signed url and can access the object
- time limited and encode all auth information
- Power UPS
  - We can create a URL for any object even if you have no acccess
  - When using the URL, the permissions match the identity which generated it
  - access denied could mean the generating ID never had access or doesnt have it now
  - Dont generate them using IamROle. as url stops working when temporary creds expire
- Demo
  - create bucket, upload an img
  - Create a signed url using cloud shell, cloud shell takes the identity of the logged in user
  - CMD: aws s3 presign s3://signedurldempo14588/aotm.jpg --expires-in 480 
  - CMD: aws s3 ls - list all buckets
  - try changing the permssions of user who created pre-signed urls, now all the pre-signed url will fail on future access
  - we can aslo generate a pre-signed url even if we don't have access to s3 or the object does not exist
  - we can also share pre-signed urls using the console

## S3 select and select glacier

- used to retreive parts of objects instead of the whole object
- s3 can store upto 5tb (huge)
- retrieving a 5 tb takes times, flitering at client side is waste
- S3/Glacier select let us use sql-like statements
- to select part of object, pre-filtered by s3
- instead of filtering at application, it is done at s3
- this saves transfter costs 
- upto 400% faster and 80 percent cheaper

## S3 events

- event notifier configuration on bucket
- Notification is generated when events occur in bucket
- can be delivered to SNS, SQS, Lambda func
- events can be such as 
  - object created (put, post, copy, multi-part)
  - delete
  - restore
  - replication (replication, failed replication)
- Event notification config
- Support only some events and services
- Event bridge suports more type of events and more services

## S3 Access Logs

- Best efforts log delivery,
- accesses to source bucket/objects are usually logged in target bucket in few hours
- it can be enabled by console ui or put bucket loggin
- managed by s3 log delivery group, we need to give target bucket access to this
- It can be done by using ACL on target bkt
- log files consist of log records , records consist of attributes
- used for audits, understand access patters of customers, billing
- we need to do our own log management


## S3 object lock

- group of related features
- Object lock enabled on new buckets (for older buckets, contant support)
- if object lock enable, versioning all enabled
- once object lock enabled, we can diable/suspend both versioning and object lock
- Object lock enable write-once-read-many (WORM) - no delete, no overwrite
- individual versions are locked
- 2 ways
  - Rentention Period
    - Specify days& years - retention period
    - Compliance mode - can't be adjusted, deleted and over written
      - until retention period expires, no one can make any changes - even the root user
      - we can change retention period on the already created objects
      - Strict mode
      - Used for complicance mode - health care/finanace
    - Governance period
      - special permissions can be granted allowing lock settings to be changed
      - s3:ByPassGovernanceRetention/x-amz0-bypass-governance-retention role can be used to over-ride
      - prevent accidental deletes
      - governance/compliance reasons 
  - Legal hold
    - Set .. object version - on or off
    - No retention
    - no deletes or changes until removed
    - s3:PutObectLegalHold is required to add or remove
      - off: normal permissions apply, we can delete
      - on: locked 
    - prevent accidental deletion of critical object versions
  - Both, one or other or none



## S3 access points

- simpilfies managing access to s3 buckets/objects
- logical breaking of a bucket so that mutilple teams can manage it easily
- rather than 1 bucket / 1 bucket policy
- we can have access points, 
  - each with different policies
  - different network access controls
  - own end point address, own dns address for network access
- created via console or cmd: aws s3control create-access-point --name "" --account-id "" --bucket ""
- matching permissions or delegation
  - main bucket policy should also need to specify the permissions
    - can have matching permssions
    - or just mention access point grant