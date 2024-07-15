

# Encryption

- [Encryption](#encryption)
  - [Intro](#intro)
  - [Client side encrytpion](#client-side-encrytpion)
  - [SSE-C](#sse-c)
  - [SSE-S3 (AES256)](#sse-s3-aes256)
  - [SSE-KMS](#sse-kms)
  - [demo](#demo)
  - [Bucket keys](#bucket-keys)

## Intro

- S3 server side encrpytion - each object is encrypted at server side
- Client side encrytpion is done at client side - all the keys are managed by them including encrypting/decrypting
- Different types - SSE-C, SSE-S3, SSE - KMS

## Client side encrytpion

- Everything is handled by client except storage
- Keys for each object, encrypt/decryption take care at client side
- s3 never sees plain text

## SSE-C

- Customer provided keys
- S3 manages enc/dec
- Offload cpu enc/dec to the s3 by saving load on our servers
- Plain text + key -> s3, encrypted + hash(key) - stored, key is ignored 
- for decryption, key is compared with hash and dec is done
- can be used in some regulated environments

## SSE-S3 (AES256)

- default
- S3 managed keys
- For each object, s3 creates a new key for each object and encrypts that object
- S3 also has a master key, used to encrypt the object key and then encrypted object key is stored in s3 storage and object key is discarded
- everything is managed by s3
- 3 problems - role seperation is not there, can't be used in heavily regulated environment, cannot manage keys
- admin's can still access the objects by using the s3 master key

## SSE-KMS
  
-  KMS Keys stored in KMS
-  KMS key, created by us, managed and it is configurable
-  s3 will use this to generate a dek and encrypt the data and store the enc data and enc key in s3
-  we can fine control over this kms key
-  role concern

## demo

- create a bucket, uplaod 2 images one using s3 and other using kms
- initially both are accessebile to iamadmin user
- update the policy to deny key usage - kms
- then only s3 image will be visible

## Bucket keys

- Put Object call sse-kms -> uses kms to generare dek -> dek is stored along with the object
- for each put, unique dek is created for each object -> kms also have cost and throttling can also occur
- Bucket keys - time limit bucket key used to generate dek's within s3, s3 automically generates dek using this bucket key
- This recudes calls to kms - reducing cost and increasing scalability
- Cloud trail kms events now show the bucket