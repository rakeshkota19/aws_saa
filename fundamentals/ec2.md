
# EC2 - Elastic Compute Cloud

- [EC2 - Elastic Compute Cloud](#ec2---elastic-compute-cloud)
  - [Basics](#basics)
  - [Life cycle](#life-cycle)
  - [Amazon Machine Image](#amazon-machine-image)
  - [Connect to EC2](#connect-to-ec2)
  - [EC2 creation](#ec2-creation)

## Basics

- Iaas - provides VM instances
- private service so uses VPC networking
- AZ resilient - Instance fails if AZ fails
- Various sizes and capabilities
- On demand billing per second
- Local on-host storage or Elastic Block store (EBS)
- CPU + Memory + Disk Storage + Networking - all these contribute to the cost

## Life cycle

- Running <-> Stopped -> Terminated/Deleted
- Stopped -> No CPu / No Memory / No Networking - only storage counts
- Terminated -> No costs

## Amazon Machine Image

- Used to create custom images or load build ec2 instance from standard AMI
- AMI - has attached permissions - public (any one can use them to create it) / owner / explicit (access for other accounts)
- AMI - Root volume
- AMI - Block device mapping

## Connect to EC2

- Linux - 22, Windows RDP - 3389
- Authenticate using SSH key

## EC2 creation

1. Go to EC2
2. Create a ssh key pair to access EC2 instance
3. Create a new instance
4. Select any Amazon AMI / free tier instance types / select key pair instance
5. Network settings/Security Group, leave them to default
6. Create the instance