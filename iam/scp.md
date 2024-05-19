
# Service Control Policies

- [Service Control Policies](#service-control-policies)
  - [Basics](#basics)
  - [Demo](#demo)

## Basics

- Policy document
- Can be attached to root container or Organisational units or any single account.
- It will applied to all the accounts under the applied ou/root.
- Managed account is not affected by SCP, which are attached to it.
- Service boundaries for any user in that container. ( Restrict what an account to do)
- By default, when scp's are enabled, everything is denied by default, unless specified.
- Both SCP and Identity policies are checked in the authorisation (Only overalapping will be authorised, rest will be rejected)

## Demo

- Currently, in Organisation, only root is there.
- Switch to prod acc, and try to upload to s3 bucket and view image. You should be able to do everything.
- Create dev and prod organisation units, and move the accounts accordingly.
- Enable service control policies
- Check full aws access polciy
- Create policy ->  denying s3 access and allowing all
- Attach this policy to PROD OU. and detach full access policy
- now, try swtiching to prod account, s3 will be denied, as Prod OU has deny s3 policy.
- We need to always atleast one policy attached, before removing any policy
  