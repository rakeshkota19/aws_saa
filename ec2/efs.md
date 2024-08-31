
# EFS

## Intro

- aws efs is an implementation of NFSv4 (only for linux)
- efs file systems can be mounted in linux
- efs can be mounted on multiple instances, and shared
- efs exist seperated from ec2
- efs is a file storage unlike ebs which is a block storage
- isolated to the vpc, accessed via mount targets
- can be accessed from outside
- posix permissions are created for efs
- efs are accessed via mount targets
- for high availability, we need to have mount targets in multiple ec2
- 2 modes
    - general purpose - default for 99 % cases
    - MAX I/O performance - for parallel, high perfomace
- Bursting and Provisioned throughput modes
- Standard and Infrequent Access (IA) classes

## Demo

