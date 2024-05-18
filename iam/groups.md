
# Groups
- [Groups](#groups)
  - [Basics](#basics)

## Basics
- Container for Iam users
- Cannot login into a Group, they have no credentials
- Solely for manegement of IAM users
- Groups can represent business teams
- Groups can also have policies attached to them ( inline / managed)
- Collect all the statemets and based on that, action is authorised
- There is no single built in all user's group account
- 300 groups per account, can be increased by support ticket
- Resource policy can't use groups directly in their policies. They can only refer to users. 
