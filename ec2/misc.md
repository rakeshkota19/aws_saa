
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
    - licensing based on physical socket/cores
    - on demand or reserved options
    - ex:  a1 host = 1 socket / 16 cores // r5 host - 2 socket / 48 cores
    - Amazon RDS instances are not supported
    - Placement groups are not suppored
    - AMI limits - RHEL, SUSE Linux, windows ami's are not supported
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

 ## Ec2 bootstrapping user data

- Bootstrap - running scripts at launch time
- Only runs these script at launch time
- will be accessed by 169.254/latest/user-data
- launch time to service time
- ec2 does not check the scripts it directly runs the script in the instance
- bootstrapping reduces the service time, by pre-running the script after launch time
- another way of reducing the service time, ami baking can be done (custom ami)
- demo of launching ecc2 with the user script in user data field
- logs can be found at /var/log/cloud-init*.log
- we can also provide this user data using cloud formation (base 64)
- cfn:init - desired script (simple configuration management script)
    - can manage any package/sources/files/commands/services
    - provided in directives in meta-data
    - can run multiple times when the meta-data is updated
- cfn:signal -
    - creation signal - create and supply a timeout value
    - instead of moving the instance to create complete status, it waits for the signal from the resource itself till it runs the user scripts(desired state)
- Creation policies create a 'WAIT STATE' on resources .. not allowing the resource to move to CREATE_COMPLETE until signalled using the cfn-signal tool.

## EC2 instance roles

- Iam role allows ec2 service to assume role and get temporary creds
- Instance profile wrapper above iamrole
- Instance profile is attached to the EC2 instance 
- Temporary credentials are accessed using instance meta-data
- creds automatically renewed
- meta-data -> iam/security-creds/role-name
- be careful while caching the temporary creds
- temporary creds/roles better to use than access keys/long term creds (when stored in unsecured environments)
- Demo:
    - For ec2 instance to access aws sdk, it needs permission
    - one way is to provide access key (not preferred)
    - create instance role in iam
    - role - AWS service/EC2, policy - s3readonlyaccess
    - in ui, when we create a role, instance profile will also be created
    - this profile will be connected to the ec2 instance
    - instnace - modify iam role, and update the role
    - latest/meta-data/iam/security-credentials/rolename
    - temporary creds are automatic renewed
- Precedence: (order of checking the creds, 1 impling it is looked first)
    1. Command line options
    2. Env variables
    3. CLI creds file 
    4. CLI config file
    5. Container creds
    6. instance profile creds

## SSM paramter store

- Systems manager parameter store
- used for storing creds/secrets securely
- passing important data through ec2-user data is bad, as everyone with access to ec2 can see it
- storage for config/secrets
- string/stringList and secure string
- license codes, database strings, full config and passwords
- hierarchies and versioning
- public parameters 
- tightly integreated with IAM
- demo
 - standard - 10k params/4kb / advanced - >10k, 8kb
 - standard free of charge
 - securestring(for storing important data) - encrypt using kms key - default ssm key / customer managed key
 - aws ssm get-parameters --names name - to get the param details
 - aws ssm get-parameters-by-path --path /my-demo-app/ - to get all params by path
 - --with-description, decrypts the encrypted as long as you have the permissions

- Instance can assume role from IAM role, gets temporary creds
- Instance profile

## System logs

- application/system logs
- cloud watch is used for managing metrics 
- does not natively caputure data inside an instnace
- we need a cloud watch agent to capture the data
- cloud watch agent need to have permission to send data to cloud watch
- agent will need some config on what to send to the cloud watch
- it also needs a role for permission to agent
- 1 log group for one log you want to inject
- Demo
    - use the stack and create the instances
    - connect to instnace
    - download cloud watch agent - sudo dnf install amazon-cloudwatch-agent
    - also give permissions to interact with s3 and ssm (config file) - cloud watch filter policy/ssm full access
    - modify role, the one which we created
    - generate config file - sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
    - use defaults until default metrics
    - enter the log file paths
    - config file - default stored at /opt/aws/cloud-watch-agent/bin/config.json
    - use the paramter store to store this
    - paramter store uses the role to create temporary creds
    - create a share collectd and types.db to store the agent data
    - start up the cloud watch agent, by providing the configuration to it

## Placement groups

- allows to influence placemnt of instances
- cluster - pack instances close together 
    - highest level of perfomance
    - launced into single AZ 
    - locked into the first instance AZ 
    - same rack  sometimes same host
    - speed between instances can be 10Bg/s, normally it is 5gb/s
    - lowest latency and max packet per second, as there are physically near
    - used when high performance is required
    - trade off - less resilient, if az falls down, then your service will fail
    - use the same type of instance (not mandatory)
    - launch at same time (not mandatory .. recommmended)
- spread - keep instances seperated
    - maximum amount of availabilty to ensure resiliency
    - seperate isolated racks in seperate zones
    - limit up to 7 instances per AZ
    - infrastructure isolation
    - each rack has own network and power source
    - not supported for dedicated instances or hosts
- partition - group of instances spread apart
    - similar to spread
    - used when you have more than 7 instances in a AZ
    - divided into partitions, max 7 per AZ (partitions are isolated from each other rank)
    - can run any number of instance in any partition unlike spread
    - designed for huge scale performance
    - resiliency + performace
    - spread + cluster benefits
    - great for topology aware applications (HDFS/Hbase/Cassandra)

## Instance roles

## Enhanced Networking 

- feature to improve networking
- SR-IOV - network card is virtualisation aware (multiple logical nic)
- without SR-IOV, there is only single interface card and host has to sit between all the instnaces and the card consuming cpu
- SR-IOV, saves cpu time and improves performace
- higher i/o and lower host cpu usage
- more bandwidth
- higher pps (packet per speed)
- consistent lower latency

<!-- ## EBS optimised 

- dedicated capacity for EBS
- enabled by default
 -->
