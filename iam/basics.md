

# IAM
- [IAM](#iam)
  - [Basics](#basics)
  - [Types](#types)
  - [Policy](#policy)
  - [IAM - New user](#iam---new-user)
  - [Access keys](#access-keys)
  - [Create Access Keys](#create-access-keys)
  - [Command Line access](#command-line-access)

## Basics

- IAM - Global service 
- Can be used to create multiple identities.
- Identities can do necessary things based on the permissions
- Also authenticates/authorise
- Only controls local identities, no direct control on external accounts or users
  
## Types

- Users - Humans or Applications that need to access
- Group - Collection of related users
- Role  - Used by AWS services or for granting external access to our acc. ( Used when number of things is uncertain)

## Policy

- Document contains set of rules tp provide/restrict access
- Can be attached to identities.

## IAM - New user

- Create easy identifiable alias name for IAM
- Create a new user
- Add AdministratorAccess policy
- Login and setup the MFA

## Access keys

- used for authentication via API's / Command line
- long term credentials (same as username / password)
- Iam User can have 2 access keys.(0,1 or 2) ( only one username/password)
- access keys can be activated/deactivated/deleted
- 2 parts, public(Access key ID)/private(Secret Access Key)
- only one time downloadable when access key is created.
- rotating keys ( create a new one for console access, and disable the old one)

## Create Access Keys

- Create Access key using IAM (Security Credentials)
- Command Line interface access
- Activate/Deactivate
- To delete, we need to deactivate
- only 2 keys at a time, we can't create more
- careful while sharing these

## Command Line access

- Install AWS CLI 2
- Use aws configure --profile name
- Create a named profile for using multiple accounts in command line
- Use us-east-1 as default region
- Check if it works using aws s3 ls --profile name
- Without providing profile name, it it will throw an error saying it can't locate creds