
# Security

- [Security](#security)
  - [Intro](#intro)
  - [Resoruce policy](#resoruce-policy)
  - [ACL](#acl)
  - [Block public access](#block-public-access)

## Intro

- private by default
- only root account/bucket created user has initial access to the bucket

## Resoruce policy

- attached to the resources
- identitiy policies - are used to restrict what users can do - only controls security in your account
- resource policies - are used to restrict what can be done on resource - can control security from other identies/accoint
- Principal part of policy - defines who the statement applies to (for identity, principle is not there, as it is the user itself)
- For identities, both identify policy and bucket policy need to allow access, for anyonymous, only bucket policy applies

## ACL

- ACLs on object and bucket
- Legacy
- Inflexible and allow simple permissions
- read/write/read_acp/write_acp, full_control
- cannot add a acl for subset of objects, not flexible
- never use

## Block public access

- added to prevent data leaks
- This feature overrides bucket policies for the public access. (only)
  
