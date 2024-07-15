
# EBS

- [EBS](#ebs)
  - [Intro](#intro)
  - [Volume types](#volume-types)
  - [SSD based](#ssd-based)
  - [HDD based](#hdd-based)
  - [Instance store volumes](#instance-store-volumes)
  - [Comparison](#comparison)
  - [EBS Snapshots](#ebs-snapshots)
  - [demo](#demo)

## Intro

- EBS - Block storage
- Raw disk allocations (volume) - can be encrypted using KMS
- EC2 create file system on top of this block system
- Storage is provisioned in one AZ ( zone resilient in that AZ)
- detached and reattached to instance, not linked to one host
- Snapshot can be taken.. Globslly resilient, migrate between AZ's/regions
- Billed on GB-month
- No cross communication between cross region instance and storage

## Volume types


## SSD based

- GP2
  - first iteration
  - General Purpose SSD
  - volume can be b/w 1GB .. 16 TB
  - io has 5.4 million IO credits 
    - ( beyond the 100 io credits, fills at 3 IO credits per second per GB .. Baseline)
    - bucket fills woth min 100 IO credits per second, regardless of volume
    - 3,000 iops burst
    - all volumes get initial 5.4 million IO credits. 30 minutes @ 3,000 iops
    - IO credit - 16 KB
    - heavy work - heavy credit usage
    - > 1 TB, baseline is above burst
  - great for boot volumes, low-latency interactive apps, dev/test
- GP3
  - SSD
  - Every volume min 3000 IOPS and 125 MiB/s
  - extra cost for < 16000 IOPS / 1,000 MiB/s
  - cheaper and faster than GP2
- Provisioned IOPS SSD
  - io1/io2/block express
  - iops can be adjusted independently of size
  - consistenct low latency and jitter
  - per instance performace
    - io1 - 260,000 IOPS & 7,500 Mb/s
    - io2 - 160,000 IOPS & 4,750 Mb/s
    - io2 Block express - 160,000 IOPS & 4,750 Mb/s
  - used when you have smaller volumes and need higher performace


## HDD based
  - moving heads - slower
  - 2 types
  - st1
    - throughput optimised
    - suited for sequential writing data
    - cheap
    - max 500 IOPS (block - 1MB)
      - baseline performace - 40MB/s/TB Base , Burst - 1250 MB/s/TB
    - 125 GB - 16TB
    - used for big data, data warehosuing, log processing
  - sc1 
    -  cold hdd
    -  in-frequent data
    -  max 250 IOPS (block - 1MB)
    -  credit pool archy -
       - baseline - 12 MB/s/TB
       - burst - 80 mb/s/tb
     - lowest cost of all EBS
     - colder data requiring fewer scans per day   

## Instance store volumes

  -  Physically connected to one EC2 host
  -  instances on that host can access these stores
  -  locally connected, so provides very high performance than ebs
  -  included in instance price
  -  they should be attached at lauch time, we cannot add them after instance launch
  -  ephemeral
  -  instacnes can move between hosts, so previous instances stores will be gone as they are connecterd to previous host 
     -  move
     -  resize
     -  hardware failure
     -  restarts
  -  instance store options provied to you depend on the instance type you are choosing
  -  D3 (storage optmised) - 4.6 GB/s throughput
  -  I3 (storage optimised) - 16 GB/s throughput
  - More IOPS and throughput vs EBS
  - high performance

## Comparison

- Persistence .. EBS (avoid instance store)
- Resilience .. EBS (avoid IS) 
- Isolated from instance lifecycle .. EBS 
- Resilience / In-built replication .. it depends
- high performace .. it depends
- Super high performance .. instance store
- Cost .. instance store 
- Misc points
  - cheap cost in EBS st1 or sc1
  - throughput or streaming - st1
  - boot .. not st1 or sc1, we can't use HDD for boot
  - GP2/3 - upto 16,000 IOPS
  - IO1/2 - upto 64,000 IOPS (block express - 256,000)
  - RAID0 + EBS up to 260,000 IOPS ( per instance performace)
  - more than 260,000 iops - instance store

## EBS Snapshots

- Backup EBS volumes to S3
- incremental in nature
- first is a full copy of data on volume, future snaps are incremental   
- Helping us to become region/globally resilient
- volumes can be created/restored from snapshots
- snaps restore lazily - fetched gradually
  - we can force a read of all data immediately
  - Or we can use Fast Snapshot restore (FSR)
    - instant restore
    - up to 50 snaps per region/AZ
    - cost extra
- Gigabyte-month cost
  - only charged for used data not allocated data

## demo