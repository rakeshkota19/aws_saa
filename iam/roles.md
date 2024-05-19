# Roles

- [Roles](#roles)
  - [Basics](#basics)
  - [Service linked roles](#service-linked-roles)

## Basics

- IAM role
- AWS services operating on behalf of us - needs access rights to do some action
- Example: AWS Lambda, might need access permissions to run a action
- To create these permissions, we create a role.
- Run time environment assumes the role, gets temporary creds, and the whole code can use this
- If role is not used, we need to hard code access keys in the AWS lambda
  -  Hard to manage ( change / rotate)
  -  Security risk
- Roles give temporary creds, and they will be deleted later after usage.
- Also when We don't know number of instances of lambda, then in these cases, we use Roles
- Roles can be added to users in emergency cases.
- Roles are used to reuse external existing identities. ( External Acc's can't be directly use AWS)
- Web Identity Federation can assume and use role and used to access AWS resources 
- Cross account usage ( We can use role, instead odf creating new users for other on-premises/partner identities) 

## Service linked roles

- Iam role linked to specific aws service
- pre defined by service, and contains all neccesary permissions required to call other services
- you can't delete the role until it's no longer required
- passing role - pass existing role into existing service

