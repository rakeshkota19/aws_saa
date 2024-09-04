
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