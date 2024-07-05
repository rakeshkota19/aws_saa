
# KMS
- [KMS](#kms)
  - [Intro](#intro)
  - [KMS Keys](#kms-keys)
  - [DEK - Data encryption keys](#dek---data-encryption-keys)
  - [Security](#security)


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
- aws owned and customer owned
- aws managed - automatically created by customer or customer managed keys - created by customer and more configurable
- all support rotation
- Aliases - refer to keys

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
- Keys should have the prinipal to trust the account
- 

