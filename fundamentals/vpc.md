
# VPC

- [VPC](#vpc)
  - [Basics](#basics)
  - [Creation](#creation)

## Basics

- Virtual network inside AWS 
- VPC's are regional - one per region - can be removed & recreated
- regional resilient
- private and isolated by default
- 2 types of VPC - default VPC and custom VPC's
- one default VPC for one region, and can have multiple custom VPC's
- default vpc are created by default and not highly configurable
- default vpc has only default IP ranges, where as custom VPC Ip's can be configured,
- default vpc - cidr ( 172.31.0.0/16)
- Subnets of VPC exist in each az zone (/20)

## Creation

- VPC
- Default VPC should be there ( Check the subnets - varies based on the zones)
