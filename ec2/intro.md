
# Intro

- [Intro](#intro)
  - [Virtualisation](#virtualisation)
  - [EC2 archy](#ec2-archy)
  - [EC2 instances](#ec2-instances)
  - [Connect](#connect)

## Virtualisation

- Run mulitple OS's on a single server
- Types
  - Emulated Virtualisation (software)
    - Hypervisor / Host OS managed the calls between other guest OS VMS and the hardware
    - Guest OS believe they have all the hardware and are on running seperate machine
    - Hypervisor does binary translation between the calls
    - slow, half the speed
  - Para virtualisation
    - modified os to call hyper calls instead of kerner calls
    - guest vm need to nbe modified
  - Hardware assisted virtualisation
    - faster
  - SR-IOV

## EC2 archy

- VM's ( OS + resources)
- EC2 instances on ec2 hosts
  - shared hosts
  - dedicated hosts
- Hosts = 1 AZ, if zone fails, host fails and then instance fails
- tied to one zone .. important
- AZ - resilient
  - Instance store - temporary storage is for each az
  - Storage
  - Data network - same AZ
  - EBS (also runs inside same AZ)
  - Volumes also runs inside same AZ
- Instances stay on host unless
  - if host fails or taken down by aws for maintenance
  - stopped and started host  (different from restart)
  - it may move into another host in the same AZ
- We cannot never connect ec2 and storage(ebs) from different zones.
- Host generally have instances of same types
- Ec'2 good for
  - os + application compute 
  - long running compute
  - server style applications
  - burst or steady-state load
  - monolithic application stacks
  - disaster recovery

## EC2 instances

- Instance types affects raw cpu, memory, local storage, type, resource ratios, network bandwidth, architecture, vendor
- 5 main categories
  - General purpose : default, steady/diverse workloads, equal resources ratio
  - Compute optimised: HPC, ML, Media processing - More CPU's
  - Memory Optimised: large in memory workloads, processing large data sets - large memory
  - Accelerated Computing: GPU, FPGA's 
  - Storage optimised: large amount of local storage - massive i/o operations - data warehousing/analytics
- R5dn.8xlarge - Instance type
  - First (R) - Instance family
  - Second - Generation ( always select the latest generation)
  - dn - collection of letters - additional capabilities 
  - 8xlarge - Instance size

![Screenshot 2024-08-23 093426](https://github.com/user-attachments/assets/d0bfca7c-ed16-42a3-b7b3-9a30367c3158)

## Connect

- EC2 SSH vs EC2 instance connect
- Ec2 instace connect uses AWS IP's to SSH into the ec2 instance

