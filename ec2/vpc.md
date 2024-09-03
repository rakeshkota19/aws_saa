
# VPC

## Intro

- Decide on IP range before creating VPC
- what size should the VPC be ?
- are there any networks we can't use 
- VPC's, Cloud, On-premises
- VPC Structure - Tiers/ Resiliency (AZ)
- VPC minimum /28 (16 IP), maximum /16 (65536 IP's)
- Personal preference - 10.x.y.z
- avoid common ranges
- reserve 2+ network per region being user per account
- 3 US, europe, australia * 2 - assume 4 accounts
- 4*5*2 - 40 ranges (ideally)

## Subnet in VPC

- Services use subnet to get the IP
- subnet will be available in one AZ
- how many AZ's to use - by default use 3-4 so atleast 4 subnets will be there
- also we can have multiple tiers, web, app, db, spare
    - so atleast take 4 tiers
- so 4*4 - 16 subnets
- if we start with /16 vpc into 16 subnets is /20
- proposal
    - 10.16 - 10.127 
    - /16 per vpc - 3AZ(+1), 3 tiers(+1) - 16 subnets
    - /16 split into 16 subnets = /20 per subnet

## Custom VPC's

- Demo - Multi tier AZ VPC's
- 12 subnets 3 for each AZ
- Internet gateway for allowing internal vpc to access public internet
- Regional resilient service - All AZ in the region
- Isolated network
- Nothing in or out without expilicit config
- Flexible config - simple or multi-tier
- default or dedicated tenancy(locked in) - be careful 
- hybrid networking - other cloud and inhouse premises
- IPV4 private CIDR & public ip's
- 1 primary private Ipv4 cidr block (min /28 to max /16)
- optional secondary ip4 blocks
- optional single assigned ipv6 /56 CIDR block
- DNS in vpc
    - provided by r53
    - vpc 'base ip+2'address
    - enable dns hostnames - gives instances dns names
    - enable dns support - enables dns resolutions in vpc

## VPC Subnets

- Subnets are generally private (depicted by blue color, public denoted by green color)
- AZ resilient
- A subnetwork of a VPC - within a particular AZ
- 1 subnet is created in a specific AZ, and it cannot be in more than one subnet
- subnet by default uses ip4 cidr, which is a subset of vpc, and it should be not overlapping with the other vpc cidr)
- subnets can communicate with other subnets from the same vpc
- 5 ip's in the subnet are always reserved
    - Network address, the first address
    - Network + 1, vpc router
    - Network + 2, dns 
    - Network + 3, reserved for future use
    - Last network address - last ip in subnet
- so total size - 5 are available for us to use
- DHCP, gives IP to the devices, for every subnet, we have dhcp server
- demo:
    - first, create a vpc, enable ipv6 cidr allocation/ dns hostname
    - create subnet by giving the ipv4 cidr and zone

## VPC Routing / Internet Gateway

- every vpc has a vpc router - ha
- runs in all zones
- in every subnet - network+1 address
- routes traffic b/w subnets
- Each subnet has route table, which tells what to do to vpc router when the packet leaves the subnet
- vpc has main route table, which acts as subnet default
- IGW region resilient gateway attached to VPC
- 1 vpc = 0 or 1 IGW, 1 IGW =  0 or 1 VPC
- runs from within AWS public zone
- gateways traffic between the vpc and the internet or aws public zone
- create and attach igw to the vpc, add entry to the route tables, add default routes sending the data to IGW
- igw keeps a mapping of public/private ip's, ec2 has no knowledge of it's public ip
- Bastion Host / Jumpbox
    - an instance in public subnet
    - used as entry point for private vpc
    - configured to permit/restrict
- Demo:
    - create a igw and attach to vpc
    - make all the subnets in web tier to access/receive public internet
    - add routes in routetable, 2 routes, one for default ip4 and ip6
    - forwarded to IGW
    - create a route table, select vpc and create
    - edit subnet associations, add the subnet-web (One subnet can be also associated with one route table at a time, default main rt)
    - edit routes, for default ip4/ip6 (0.0.0.0/0), forward to internet gateway
    - edit the subnet settings for web tier, click on enable allocate public ip4 address


## Stateless vs Statefull Firewall

- Stateless
    - 2 rules (1 in, 1 out) - Inbound application
    - 2 rules (1 out, 1 in) - Outbound updates
- Stateful
    - can identify tjhe request/respone of a connection as being related
    - stateful means lower admin overhead

## NACL - Network access control list

- stateless
- L4
- NACL's filter traffic crossing the subnet boundary (inbound / outbound)
- inbound rules affect only the inbound req / and vice versa
- NACL offer both excplict deny and allow
- rules match the dst ip/range/port/protocol and allow/deny based on that match
- processed in order, lowest rule number first, once a match occurs processing stops, if nothing matches it's implicit deny
- stateless, both inbound and outbound need to be seperate
- lot of rules, if there are multiple tiers
- A VPC is created with a default NACL, all traffic is allowed implicitly
- not affected if communication is happening in the same subnet
- can only be assigned to subnets, not to any aws services

## Security Group

- Stateful - detect response traffic automatically
- no explicit deny traffic using sg only allow or implicit deny
- L7
- Support Ip/CIDR and logical resourcces
- attached to ENI's not instances  / subnets
- for ec2, sg is associated with the primary ENI of the instance
- response flow is automatically allowed if request is allowed
- also allows the usage of logical resources
- we can refer to the other sg groups, in the in-bound/outbound rules, all the instances using that relevant sg can use this rule
- we can also use self references, anything with the same sg can communicate with other instances

## Network Address Translation and NAT Gateway

- NAT, set of processes - remapping src and dst ip's
- Static NAT - IGW stores mapping of private/public ip, and uses this to send data from instnace to the internet.
- IP Masquerading - hiding CIDR blocks behind one IP
    - Public Ip4 addresses running out
    - gives private cidr range outgoing internet access
- NAT Gateway
    - Runs from a public subnet
    - uses elastic ip's (static IP4 public)
    - AZ resilient service (HA in that AZ)
    - for region resilience - NATGW in each AZ ( RT in for each AZ with NATGW as target)
    - Nat instance vs Nat gateway
        - nat gateway - high performace/highly scalable/ custom designed
        - nat instance - single ec2 instance in AZ/ ha in AZ
        - cost - low volume/test instance nat instance can be cheaper
        - nat gateway will be scaled automatically, not free tier eligible
        - nat instance - can use for multipurposes as there are ec2
        - nat gateway can't act as baston server, can't port forward
        - nat instnace we can filter network using security group / nacl
- nat is used only for ip4, not for ipv6, all ipv6 addresses are publicly addressable
- nat gateways don't work with ipv6
- ::/0 route + IGW - bidirectional connectivity
- ::/0 route + egress-only IGW - only outbound



