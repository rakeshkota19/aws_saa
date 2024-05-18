

# Users
- [Users](#users)
  - [Basics](#basics)
  - [ARN](#arn)
  - [Limitations](#limitations)

## Basics

- Users -> Identity used for anything requiring long term aws access ( Humans/Applications/Service accounts)
- 2 step process - Authentication + Authorisation
- First users authenticate themselves using username/pwd or access keys
- After words, whenever they try to access any resource, policies are checked and necessary authorisation is done
- New Iam User has no permissions, so implicit deny applies

## ARN

- Amazon resource number
- Uniquely identify resources within any AWS acc
- Used to refer resources when using console / api's
- arn:partition:service:region:account-id:resource-id
- arn:aws:s3:::bucketname -> refers to the bucket itself
- arn:aws:s3:::bucketname/* -> refers to all the objects in the bucket iself
- * can be used as wildcard in arn, it will refer to any of the value for the corresponding field

## Limitations

- 5000 IAM users per account
- Iam user can be a member of 10 groups
- Has systems designs impacts
- IAM users doesn't work for internet scale applications
- IAM roles and Identity Federation fix ( these are used for more than 5000 users)