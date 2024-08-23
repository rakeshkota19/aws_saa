
# Storage

- [Storage](#storage)
  - [Intro](#intro)
  - [Storage performace](#storage-performace)

## Intro

- Types of storage
  - Direct (local) attached storage - storage on the ec2 host
    - lost if disk fails
    - lost if instance moves to a different host
    - very fast
  - Network attached storage - volumes delivered over network (EBS)
    - highly resilient
    - sepeate from ec2 host
  - Ephemeral storage - Temporary storage 
  - Persistent storage - lives on past the lifetime of instance
- Block storage
  - volume presented to OS as collection of blocks
  - backed up by hdd/ssd
  - no inbuilt structure, os builds the file system (ntfs // )
  - collection of uniquely addressable blocks
  - mountable, bootable
- File storage
  - presented as a file share, has strucutre, mountable, not bootable
  - ready made file system, already have structure
  - accessed by multiple instances simultaneously.
- Object storage - colleciton of objects, flat. (Not mountable, Not bootable)
  - Super scalable 

## Storage performace

- IO (block size)
  - size of wheels
  - size of blocks writing to disk
  - 16KB .. 64 KB .. 1 MB  
- IOPS
  - revolutions of the car / speed of car
  - no of io opeations per second 
  - can be affected by network drives, hardware technology
- Throughput
  - speed of the car
  - t = io * iops
  - increase io/iops can increase the throughput
  - expressed in mb/s
