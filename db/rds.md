

# RDS

## Intro

- Different types of Database (SQL/NOSQL)
- Acid VS Base
- rds limits scaling
- basically available soft state eventual consistency
- base are highly available
- dynamo db is also base but it also has transactions for consistency

## Database on EC2

- Single Instance - all on same server
- Mulitple instnace - web/application/database in different instance
    - decide whether they are between same AZ are not
    - data transfer cost between AZ's is billed
    - no data transfer costs inside the same AZ
- Only run if you need to
    - OS DB access 
    - root level access for advanced db option tuning
    - vendor demands
    - db or db version on AWS is not there
    - architectures that AWS don't provide (replication/resilience)
- never use unless these conditions
- reasons for not putting
    - admin overhead - managing ec2/db host
    - backup/dr management
    - ec2 is single AZ so database will run from single instance .. point of failure
    - some of aws products features are amazing, and are very performant
    - not very easy to scale
    - replication - monitoring, setup time - overhead

## Demo 

- Moving from monolithic to the multiple instance
- Click on the one click deployment (creates two instances, one for wordpress and one for maria db)
- take a dump and restore it in the database instance
- update the wp-config file with the db host
- everything should work after updating the host

## RDS architecture

- not Database as a service (DBaas) 
- Database server as a service(DBSaas)
- multiple databases on one db server (instance)
- we can pick any different db engines
- it is different from aurora
- managed service .. no access to OS or ssh (some rds have low level access)
- one db subnet group for one rds (more flexible)
- dedicate storage ebs per instance
- primary / standy will be on different AZ (replication occurs between them)
- rds can also be accessed with public addressng or private vpc
- cost archy
    - cost depends on instance size / type
    - multi az or not 
    - storage type and amount - per gb
    - data transferrer - outside aws or region costs
    - backups and snapshots (per gb per month)
    - licensing
- we dont see any ebs, ec2 instances, everything is managed by the aws

## Demo, provisioning RDS

- RDS, first create a subnet group, informs rds which subnets to use for database instance
- it's how rds, determines which subnets to place rds instances into
- using this, placement of rds instnaces happen
- create a database
- standard create 
    - sql: mysql
    - templates: defaults based on the environment 
    - multi AZ cluster not possible for free-tier
    - input the db creds
    - pick the instance / storage
    - select the vpc/subnet group
    - we need to choose vpc group(security group) for restricting/allowing accesss to the instance
    - create the database using all defaults
- all rds instances are given a endpoint and port
- configure security group for the vpc
- use the dump and update wp config host
- delete the db/stack/subnet group/vpc security group

## Multi AZ - Instance

- option for providing HA
- primary instance is replicated to standy instances in different AZ (synchronous)
- replication occurs through storage level, less efficient than cluster
- CNAME always points to the primary database, it only updates when primary goes down
- backups can occur from standy to S3
- fail over occurs if the primary fails
- 60 to 120 seconds to update during failover
- extra cost for replica
- one standy by replica 
- cannot be used for read/write
- same region only,  different AZ
- AZ outage, primary failure, manual failover, patching

## Multi AZ - Cluster

- capable one write can replicate to 2 readers (unlike aurora, where we can multiple slaves)
- readers are usable unlike standy 
- writers commits data, when atleast one of the readers update the data
- synchronous replication
- Endpoint
    - Cluster endpoint: Pointed at the writer, used for reads, writes and administration
    - Reader endpoint: directs any reads at available reader instance
    - Instance endpoint: generally not used, only for testing/fault tolerancy
- 1 writer and 2 reader
- much faster archy - graviton + local NVME SSD storage
- faster writes to local storage => flushed to EBS
- replication is via transaction logs
- fail over is faster - 35s + transaction log apply

## RDS backup

- two types of backups
    - automated backups
    - snapshots
- they are stored in aws managed s3 buckets 
    - regionally resilient 
- backups are taken through standby instances
- Snapshots:
    - aren't automatic
    - we need to run them
    - stored in s3
    - all the databases will be there
    - first time it is full copy
    - later it is incremental 
    - don't expire, we need to clean them manually
    - lower the time between snapshots, the less data will be lost in case of any issue
- Automated backups:
    - occur every 1 day
    - we can pick the time period
    - first it is full, then it is incremental
    - for every 5 minutes, transaction logs are also backed up
    - they are automatically cleared by rds
    - retention period: 0 to 35 days (0 means automated backups turned off)
- RDS can also replicate snapshots to another region
    - both snapshots/transaction logs
    - cross-region charges apply
    - not default, need to be configured

## RDS restore

- when we restore, a new instance of rds will be created
- snapshots, single point in time, creation time
- using automated, we can restore to any 5 minute point, first full is restored, then incremental, then transaction logs are replayed
- restores aren't fast

## RDS read replicas
- High performance / resiliency
- asynchronous replication is used to sync unlike standby
- can be created in the same region/different region
- we can create 5 read replicas per db instance
- read replicas can also have read replicas but lag starts to be a problem
- global improvements
- RR's offer 0 RPO
- RR's can be promoted quicky - low RTO
- read only - until promoted
- global resilience
- can only used for fail over case, cannot be used for corruption case

## Demo (Multi AZ and restore)

- Use the stack
- Create a wordpress
- take a snapshot of the database, first snapshot can take time
- next snapshots will be faster as there are incremental
- snapshots taken manually need to be cleared by us
- enable multi az to convert single instance to multiple instances
    - create a standy instance (creates in a seperate AZ)
    - primary instance is syncronously replicated to the standy instance
- reboot with failover to simulate fail over
    - cname will be updated to the standby
- restore creates a new rds instance, does not use the old instance
    - need to update the database url in the word press
    - brand new end point

## RDS data security

- SSL/TLS in transit is available for RDS, can be made madatory
- RDS supports ebs volume encryption - KMS (AWS or Cusomter managed keys)
    - dek's are loaded into the host as required
    - data is encrypted and decrypted at the db instance host before it is transferred or recieved
- encryption cannot be remove once added
- RDS MSSQL/RDS Oracle also supports TDE
    - Transparent data encryption, encryption is handled inside the DB engine
- RDS oracle also supports integration with cloud hsm, which is much more stronger than AWS
    - removing aws from chain of trust
- IAM auth for database
    - Policy attached to users or roles maps the IAM identity onto the local RDS user
    - generate-db-auth-token, with a 15 minute validity can be used to auth
    - iam is only for auth, it can't authorise

## RDS custom

- fills the gap between RDS and EC2 running a DB engine
- RDS is fully managed by AWS
- this offers middle ground between the two
- currently works for MS SQL/Oracle
- we can see all the ec2 / ebs volumes and backup
- rds custom database automation

## Amazon Aurora

- very different from rds
- uses a cluster
- a single primary instance + 0 or more replicas
- unlike rds, these replicas can be used to read data from
- no local storage - uses cluster volume
- replication occurs at the storage level, so it happens very fast in cluster volume
- storage is much more resilient than RDS
- upto to 15 replicas
- All SSD based - high iops, low latency
- storage is based on what you consume
- high water mark - billed for the most used (max storage)
- storage which is freed until high mark can be re-used
- replicas can be added or removed without requiring storage provisioning (cluster volume)
- use endpoints
    - cluster endpoint - primary
    - reader endpoint - loadbalanced among read
- no-free tier option (micro instances are not supported in aurora)
- compute - hourly charge per second
- storage - gb/mnth consumed, io cost per req
- 100 % db size in backups are included
- backtrack can be used which allow in-place rewinds to a previous point-in-time
- fast clone, make a new database much faster than copying all data
    - keeps only the changed data
    - same data will be referenced

## Aurora serverless

- don't need to provision server
- serverless cluster has min and max ACU
    - Aurora capacity units
    - allocated from a pool
    - stateless and can be allocated very fastly
- cluster adjusts based on load
- can go to 0 as well
- consumption billed per-second basis
- same resilience as Aurora (6 copies across AZ's)
- share proxy fleet (managed by aws)
    - create a connection between application and database
- good for
    - infrequently used applications
    - new applications
    - variable workloads
    - unpredictable workloads
        - normal aurora provisioned will waste resoruces when load is very small
    - multi-tenant applications

