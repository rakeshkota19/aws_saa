
# KMS
- [KMS](#kms)
  - [Intro](#intro)
  - [KMS Keys](#kms-keys)
  - [DEK - Data encryption keys](#dek---data-encryption-keys)
  - [Security](#security)
  - [Demo](#demo)


## Intro

- Used by all other aws services
- Regional and Public service
- Manages keys (both sym and asym)
- Performs crypt operations
- Keys never leave KMS ( FIPS 140-2 (L2) - US standard, most cloud providers is only L1 compliant)

## KMS Keys

- Logical - ID,date, polciy, desc and state
- backed by physical key material 
- generated or imported
- KMS keys can be operated on data upto 4 kb of data
- isolated to a region and never leaves 
- multi region keys are allowed using replicaton
- aws owned(aws services manages and owns them) and customer owned
- aws managed - automatically created by aws when we use aws services such as s3 or customer managed keys - created by customer and more configurable
- all support rotation, aws managed cannot be disabled, customer owned, rotation is disabled)
- Aliases - refer to keys
- all previous backing keys will be stored so that previous versions can be accessed

## DEK - Data encryption keys

- GenerateDataKey - works on > 4kb
- not stored in any way, it is provided to the user ( plain text version, cipher text (encrypted using kms))
- encrypt data using plain text version / cipher text is stored along with this data
- plain key is discarded after the use
- same dek can be used multiple times 
- pass the cipher text -> returns the plain key  -> We can decrypt the file
- S3 created a new DEK for each object and discards the plain key

## Security

- Key policy (similar to resource)
- Every key has policy
- Keys should have the principal to trust the account

## Demo
- KMS -> Customer manged - create key
- Define administrative permissions - used to manage key (crud)
- Define key usage permissions - used to define who can use key
- Create policy
- Key rotation can be enabled, by default not enabled
- Keys are rotated generally once every year