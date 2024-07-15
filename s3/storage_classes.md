
# Storage classes
- [Storage classes](#storage-classes)
  - [S3 standard](#s3-standard)
  - [S3 Standard - IA (Infrequent Access)](#s3-standard---ia-infrequent-access)
  - [S3 standard - One Zone IA](#s3-standard---one-zone-ia)
  - [S3 Glacier - Instant](#s3-glacier---instant)
  - [S3 Glacier - Flexible (also known as S3 Glacier)](#s3-glacier---flexible-also-known-as-s3-glacier)
  - [S3 Glacier - deep archive](#s3-glacier---deep-archive)
  - [S3 Intellgent-Tiering](#s3-intellgent-tiering)

## S3 standard

- replicated across atleast 3 AZ
- durable and resilient
- 11, 9's durable
- charged on GB/month for data stored, transfer out fee and price for requests
- No minimum duration or no minimum size fee
- first byte latency - milliseconds
- used for frequently used data, not replacable
- default option

## S3 Standard - IA (Infrequent Access)

- shares most of the S3 standard 
- replicated across atleast 3 AZ
- first byte latency - milliseconds
- storage costs are half of S3 standard
- New cost - retrieval fees for every GB, minimum duration charge of 30 dats, minimum capacity charge of 128KB per object
- used for long lived data, not used frequently and important data


## S3 standard - One Zone IA

- Only in one zone (still replicated in the zone)
- used for long lived data, not used frequently and unimportant data

## S3 Glacier - Instant

- Similar to Standard IA
- instant retreival but very less frequently used
- Minimum charge of 90 days, 128KB per object
- long lived data accessed once per qrtr with millisecond access
- expensive retreival

## S3 Glacier - Flexible (also known as S3 Glacier)

- 3 AZ
- Same durability characterstics
- 1/6 standard cost of s3 standard
- cold objects .. not warm .. not immediately available
- cannot be made public accessible .. any acess requires retreival process
- retreival process - expedited (1-5 minutes), standard (3-5 hours), bulk (5-12 hours), faster = more expensive
- archival data where frequent / real time is not used


## S3 Glacier - deep archive 

- Cheapest archive
- 3 AZ
- retreival process  standard (12 hrs), bulk ( < 48 hrs>)

## S3 Intellgent-Tiering

- Frequent Access/Infrequent Access/Archive Instant Access/Archive Access/Deep Archive
- S3 automatically monitors and moves the objects based on their usage
- 90 days minimum to move to archive instant access
- configurations can be set on buckets based on prefix and tags
- intelligent tiering automation cost per 1,000 object
- only good, if the usage patterns change or you dont know