
# Hybrid Env

## Border gateway protocol - BGP

- Routing protocol
- Autonomous systems (AS) - routers controlled by one entity - a network in BGP
- ASN are unique and allocated by IANA (0-65535), 64512 - 65534 are private
- BGP operates over tcp/179 - reliable
- not automatic - peering is manually configured
- BGP is a path-vector, it exchanges the best path to destination .. ASPATH, only based on paths, not on latency 
- IBGP - internal BGP, routing within an AS
- EBGP - External BGP, routing between AS's
- always shorter path is preferred
- ASPath with shorter path is preferred irrespective of the actual path (fibre link/satellite)
- We can force bgp to use a path by using Aspath prepending (202,202,202, i) -> making the path bigger (3hops)

## IPSEC Vpn Fundamentals

- Group of protocols used to secure tunnels across insecure networks
- between 2 peers
- provide auth
- data is encrypted
- interesing traffic - packets maching certain rules -> tunnels are created for this packet else they are torn down
- encryption
    - symmetric enc is fast, most cpu's can do it with less overhead, but the communication to exchange keys is a challenge
    - asymmetric enc is slower, so it is used to exchange symmetric keys and later use symm enc
- it has 2 phases
    - IKE Phase 1 (Slow and heavy) 
        - authenticate (done using pre-shared key/certificate)
        - uses asym enc to agree on, create a shared symm key (dh private key)
        - dh key is generated independently
        - each side generates a symmetrical key
        - ike sa created (phase 1 tunnel)
    - IKE phase 2 (fast and agile)
        - uses keys agreed in phase 1
        - agree enc method, keys used for bulk data transfer
        - create ipsec sa .. phase 2 tunnel (running over phase 1)
        - torn down when interesting traffic is stopped
- IKe phase 1 is normally used after auth and sym key generation
- Ipsec key is used for bulk enc and dec of interestnig traffic
- Policy based vpn's
    - rule sets match traffic / different rules for security settings
- Route based VPN's
    - match a single pair of SA's / matches path / easier to setup 

## AWS site to site VPN

- logical connection b/w vpc and on-premises network encypted using IPSec
- Full HA, if you implement correctly
- Quick to provision, less than 1 hr
- Virtual Private gateway (VGW)
- Customer Gateway (CGW)
- VPN connection b/w VGW and CGW
- Static connection, you need to tell ip ranges
- CGW is single point of failure, at AWS side, private GW has two endpoints in two seperate regions
- We can add another CGW at customer in another building, ensuring HA at both ends
- static vs dynamic vpn (BGP)
    - dynamic uses BGP
    - static vpn uses static networking, simple
    - static does not have load balancing / multi connection failover
    - dynmaic, bgp routes are communicated
    - route propogation, enabled, routes are added to Route table's automatically
- speed cap for vpn - ~1.2 Gbps
- more data, more enc/dec
- latency considerations, inconsitent, public internet
- cost - aws hourly cost, GB out of cost, data cap
- very fast to setup 
- can be used as a backup for direct connection (DX)

## Demo

- [Git link](https://github.com/acantril/learn-cantrill-io-labs/tree/master/aws-simple-site2site-vpn)

## Direct connect (DX)

- A physical connection (1 GB, 10 or 100 Gbps)
- Business premises -> DX location -> AWS region
- Port allocation at a DX location 
- port hourly cost and outbound data transfer
- provisioning time .. physical cables and no resilience (weeks/months)
- low and consistency latency + high speeds
- used to access private services(vpc) and aws public network
- DX location - not owned by AWS, large regional data centre, aws has space and equipment
- If you don't have space/equipment at a DX location, a comms provide can extend the DX port into your business
- Cross connect AWS port to customer/partner port link at DX location
- Resilience and HA
    - DX locations are connected to AWS region via redundant high speed connections
- 7 single points of failure
    - DX location
    - DX router
    - Cross connect cable
    - Customer DX router
    - Extension from DX location to customer premises
    - Customer Premises
    - Customer Router
- To make it HA
    - get multiple DX ports
    - multi cross connects cable
    - multi customer dx router
    - multi customer router
- still single point of failure
    - dx fails
    - same cable path between locations fails
- To make it better HA
    - two DX locations (to make it more resilient, we can mulitple ports in single DX loc, and have multiple customer router in one premise)
    - two customer premises
- Public VIF (virtual interfaces) + VPN
    - Encrypted & Auth tunnel
    - Over DX (low latency & consistent latency)
    - VPN - Transit Agnostic (DX/Public internet)
    - VPN has more cryptographic overhead vs Macsec which limits speeds

## Transit Gateway

- Network transit hub to connect vpc's to on premises network
- reduces network complexity
- single network object - ha and scalable
- vpc, site to site vpn
- 4 vpc's, noraml to connect all of them using mesh network, we need 6 connections
- we can use transit gateway to connect these 4 vpc's
- supports transitive routing
- can be used to create global networks
- peer with different regions (same or cross account)
- less complexity when used

## Storage gateway volume

- Virtual machine
- presents storage using iscsi, nfs or smb
- integrates with ebs, s3 and glacier within aws
- migrations, extensions, storage tiering, dr ans replacement of backup systems
- Volume stored
    - raw block devices presented per iscsi
    - all are stored locally
    - temporary : upload buffer and async updated to the aws (ebs snapshots)
    - great for full disk backups (can be easily restored, DR)
    - dont improve data center capacity, main copy of data is stored on gateway
- Volume cached
    - same architecture as volume stored
    - main location is not data center, it is s3 (ebs snapshots)
    - only frequently accessed data is stored in local 
    - aws becomes extension of the data center
- VTL Mode (Tape)
    - Large backups - tape 
    - Tape - sequential read/write
    - Normally inpremises or offsite tape storage are used for backup
    - VTL - virtual tape library backed by s3 and tape shelf (vts) stored in glacier
    - virtual tape 100 GB - 5TB
    - unlimited vts (archive) storage
- File
    - Bridges on-premises file storage and S3
    - mount points available via NFS or SMB
    - map directly to s3 bucket
    - s3 objects visible as on-premises files
    - primary data is held in s3
    - aws service integration using events and lambda
    - notify when uploaded, event is sent to update other gateways storage when something is uploaded
    - file gateway doesn't support object locking - use read only mode on other shares or tightly control file shares
    - default is s3 standard, life cycle can move objects around


## Snowball and Snow mobile

- designed to move large amount of data in and out of aws
- physical storage .. suitcase or truck
- ordered from aws empty, loadup, return or order with data, empty, return
- Snowball
    - not instant, order and device delivered
    - data encryption uses kms
    - 50TB or 80 TB
    - 1 gbps (rj45 1GBase-TX) or 10 Gbps (LR/SR) network
    - 10 TB to 10 PB economical range (multiple devices)
    - multiple devices to multiple premises
    - only storage
- Snow ball edge
    - both storage and compute
    - larger capacity compared to snowball
    - 10 Gbps (rj45), 10/25 (SFP), 45/50/100 Gbps (QSFP+)
    - 3 types
        - storage optimised - 80TB, 24 cpu, 32Gib Ram, with ec2 - 1TB SSD
        - computed optimised - 100 TB + 7.68 NVME, 52 vcpu, 208 Gib ram
        - compute with gpu - with gpu
- Snow mobile
    - Truck, container with portable data center
    - Ideal for single location when 10 PB+ is required
    - Up to 100 PB per snow mobile
    - not economical for multi site or sub 10 PB

## Directory service

- Managed service
- stores objects (Users, groups, computers, server, file shares) with a structure
- commonly used in Windows environments
- Microsoft AD DS / Samba
- Runs within a VPC
- HA, deploy into multiple AZ's
- amazon workspaces, some aws services need a directory
- Mode
    - Simple AD mode (Stand alone directory which uses samba 4) 
        - 500users(small)/5000 users (large)
        - integrates with aws services
        - not designed to integrate with any exisiting on-premises directory system such as Microsoft AD
    - AWS Managed Microsoft AD
        - more features than simple AD mode
        - can be integrated with on-premisees directory system
    - AD connector
        - one specific aws service has requirement of directory
        - you already have directory in on-premise and you dont want to create new service
        - acts as proxy
- Pick simpe AD, simple requirements, if you need MS AD DS or need to trust AD DS, use AWS managed microsoft ADS

## Data Sync

- Data tranfer service to and from AWS
- Migrations, data processing transfers, archival, dr/bc
- designed to work at huge scale
- keeps metadata (permissions/timestamps)
- built in data validation
- Scalable - 10Gbps per agent (~100 TB per day)
- Bandwidth limiters
- Incremental and Scheduled transfer options
- compresion and enc
- automatic recovery from transit errors
- aws service integration - s3, efs
- pay as you go - per gb data involved
- we can configure scheduler and also customer impact can be minimised by setting a bandwidth limit
- can be integrated to efs, s3, fsx
- components
    - task
    - agent
    - location

## Fsx for windows file server

- Fully managed native windows file servers/shares
- used active directory (managed or self managed ad (on-premises))
- designed for integration with windows environments
- single or multi-az within a vpc
- on-demand and scheduled backups
- accessible using vpc, peering, vpn and direct connect
- Native windows file system supports distributed file system, kms at rest and forced encryption in transit
- supports volume shadow copies (file level versioning)
- vss - user driven restores (view previous versions and restore without admin)
- native file syste accessbile over SMB (if SMB, mostly it is FSX)
- Windows permission model
- supports DFS .. scale out file share structure
- Managed, no file server admin
- integrates with DS and your own directory

## Fsx for lustre

- Manged lustre - designed for HPC - Linux clients (POSIX)
- ML, Big data, Financial Modelling
- 100 Gb/s throughput / sub millisecond latency
- deployment types - persistent or scratch
- scratch - highly optimised for short term, no replication and fast
- persistent - longer term, HA (in one AZ), self healing
- accesssible over vpn or direct connection
- Data can use S3 and lazy load data only when data is needed
- for scratch - base 200 MB/s per TB storage
- persistent offers 50Mb/s, 100Mb/s, 200Mb/s per Tb of storage
- Burst upto 1300 MB/s per Tb (credit system)
- lustre runs from one AZ
- we can backup to s3

## AWS transfer family

- Product, managed file transfer used to transfer files to and from (s3, efs)
- FTP - unencrypted file transfer
- FTPS, SFTP, Applicability Statement 2 (AS2) - Structured B2B data
- supports identity providers
- managed file transfer workflows - serverless file workflow engine
- multi AZ - resilient and scalable
- Provisioned server per hour $ + data transferred $
- FTP - VPC only (cannot be public)










