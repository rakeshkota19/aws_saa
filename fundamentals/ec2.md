
# EC2 - Elastic Compute Cloud

- [EC2 - Elastic Compute Cloud](#ec2---elastic-compute-cloud)
  - [Basics](#basics)
  - [Life cycle](#life-cycle)
  - [Amazon Machine Image](#amazon-machine-image)
  - [Connect to EC2](#connect-to-ec2)

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