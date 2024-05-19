
# Organisations

- [Organisations](#organisations)
  - [Basics](#basics)
  - [Demo](#demo)

## Basics

- AWS service used to manage multiple of AWS accounts.
- Each AWS account has their seperate users/payment info. Normally it is called Standard AWS account
- Management account/Master account - AWS account where organisation is created
- Using this management account, we can invite the standard aws accounts.
- After joining, standard aws accounts become member accounts
- Nested Strucutre
  - Organisation root can contain many organisational unit (OU)
  - AWS structure can be similar done to business structure
- Payment Info will be removed from member account, after joining and it will be moved to management account
- Only one bill in Management account/Payer account
- Consolidation of reservations and volume discounts
- We can also create new account in the organisation itself. 
- Single AWS account to login into the member accounts, instead of having seperate iam user for each account
- OrganizationAccountAccessRole can be used to access member accs.
- When a new memner is created, this role will be automatically created.
- When a member is invited/joined, then we need to create a role to switch to acccess member acc from master acc.

## Demo
- Creation of organisation using gen account
- Inviting  member (prod account)/ creation of new member acc(dev account)
- Adding a new role and switching role on the management account to acces the member account
- We can access dev/prod accounts from the Management account using switching role.