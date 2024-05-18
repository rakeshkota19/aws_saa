
# Policies
- [Policies](#policies)
  - [Basics](#basics)
  - [Statement](#statement)
  - [Overlapping Rules](#overlapping-rules)
  - [Policy types](#policy-types)

## Basics

 - Set of rules/statement
 - Grants/denies Permission
 - Json Doc
 - Collection of statements applying to indenties
 - When user tries to access any resource/perform any action, aws will collect all the statements and check

## Statement

- Sid - Optional, used for identification purpose / description
- Effect - Allow/Deny
- Action - action ( service:operation or wild cards)
- Resource - arn format
- Only runs the effect if action and resource matches

## Overlapping Rules

- Allow S3, deny certain bucket - which will take effect ?
- Priority is preferred in case overlapping issue
  - 1. Explicit DENY ( More priority)
  - 2. Explicit ALLOW
  - 3. Default DENY (Implicit) - AWS by default denies
  
## Policy types

- Inline Policy
  - Applies JSON to each account individually
  - Not best practise
  - Need to change multiple times
  - Special or exceptional allow or deny 

- Managed Policy
  - Attach to any identies
  - Reusable 
  - Low Management Overhead
  - AWS managed policy / Customer defined Managed policy