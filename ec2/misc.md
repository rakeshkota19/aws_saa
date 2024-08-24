


# Misc

## Network / DNS Archy

- EC2 can have multiple Elastic Network Interfaces, however all should be in same AZ
- Each NI has 
    - mac
    - 1 primary private IP4 .. static .. donot change for life time of instance .. dns (ip.ec2.internal .. only resolve inside private region)
    - 0 or more secondary IP's
    - 0 or 1 public IP4 - changes after start/stop (restart won't stop) - (ec2-ip.compute-1.. -> redirects to private ip inside vpc else public ip)
    - 1 elastic IP per private IPv4 - attached to instance (when attached, non elastic public ip will be removed)  
    - 0 or more ip6 (all are publicly routable)
    - security groups attached to interface
    - source/destination check
    - secondary ni can be detachable

- Secondary ENI + MAC = Licensing -> can we moved between instances by attaching/detaching ni
- Multiple interfaces vs Multiple IPs
    - Different security groups for multiple interfaces
- OS - Doesn't see public IPv4
- Normal public IP4 are Dynamic 
- public DNS = private IP in VPC, public IP everywhere else

## AMI

- used to launch instances
- aws provided or community provided
- market place for commercial software ( extra cost can be there for license)
- Regional and each has unique ID
- We can also create an ami image from our current instance
- When we create AMI for an EBS volume - snapshots for volume are taken and are referenced inside AMI (Block device maping)
- When we create a new instance from AMI, the earlier volumes will be reattached to the instance
- AMI's are regional, EBS are regional
- AN AMI can't be edited .. launch instance, update config and make new AMI
- Can be copied between regions (include its snapshots)

### Purchase Options

- On Demand
    - Run on shared hardware, multiple instances can run on a host
    - per second billing for running time (other resources can be charged even if ec2 is down like storage)
    - default purchase optioon
    - pay as you go
    - no capacity reservation / predictable pricing / no upfront cost / no discount
    - short term/unknown workloads
- Spot 
    - Cheapest (upto 90 percent discount)
    - AWS sells ununsed ec2 host capacity
    - prices varies and it set by AWS based on the resources
    - if prices are more than your max price, your spot instances will be terminated
    - instances can be terminated and interruptions can occur
- Reserved
    - we can reserve instance for 1/3/5 year
    - can be locked to AZ/region
    - unused reservation still billed
    - partial coverage of larger instance
    - cheaper than on demand
    - no upfront + per-s fee / partial upfront + reduced per-s fee / all upfront + no per-s fee (cheapest) 
    - Scheduled reserved
        - used for long term usage and doesn't run constantly
        - book instance for specific time/resources
        - use the resource only for that time
        - cron jobs/ batch processnig/weekly analysis
        - minimum time
- Dedicated Hosts
    - pay for the dedicated hosts instead of shared hardware
    - no per second charge for the instnace
    - we pay for the host, so we can use all resoruces
    - it also has host affinity - hosts will be running on same host
    - licensing based on socket/cores
- Dedicated Instances
    - we dont pay for the host
    - AWS runs all your instances in a single instance
    - extra charges
- Capacity reservations
    - Regional reservation .. book resoruces for that region
    - Zonal reservation .. book in only one AZ
- EC2 Savings plan
    - hourly commitment for a 1/3 year term
    - general compute savings (Ec2, lamda, fargate) - commit to a amount and once crossed, on demand prices apply
    - EC2 savings plan - flexibilty on size and OS

## Instance status checks 

- 2 status checks 
    - system status check .. issues with the Ec2 instance/host
    - instance status check .. os kernel issues, networkign issues, corrupted file systems
- Auto recovery moves system to a new host and starts with the same settings
- Create status check alarm - alarm action - recover/reboot
- only can be done for certain type of instances

## Termination Protection

- Prevent terminate instance option by enable termination  protection
- turn of the protection to terminate the ec2
 
## Instance Metadata

 - EC2 serice provides data to instances
 - accessible to all instances
 - IP : 169.254.169.254 (/latest/meta-data)
 - all the environment information
 - netowrking
 - authentication (uses metdata to create temporary creds)
 - user-data
 - no authenticated and not encrypted 
 - DEMO Commands [Link](https://learn-cantrill-labs.s3.amazonaws.com/awscoursedemos/0009-aws-associate-ec2-instance-metadata/lesson_commands.txt)
 - We can also use ec2-metadata package to get data