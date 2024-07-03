

# Control Tower

- [Control Tower](#control-tower)
  - [Basics](#basics)
  - [Control tower](#control-tower-1)
  - [Guard rails](#guard-rails)
  - [Account Factory](#account-factory)

## Basics

- Quick/Easy setup of multi - account environment
- Orchestrates other AWS services to provide this functionality
- Organisations, IAM Identity Center, Cloud formation, Config
- Next evolution of AWS organisation 
- Landing zone - multi account environment
- Single SSO/ID Federation, Centralised logging and auditing
- Guard rails - detect/mandate across all accounts
- Account factory - automates and standarises new account creation
- Dashboard

## Control tower

- Control tower in Management account
- Creates AWS organisations
- Creates 2 OU, foundational/secutiry and custom/sandbox
- In foundational - 
  - 2 accounts 
  - audit account ( used for users needing to access -audit data - SNS / Cloud Watch) 
  - log archvie account (used for reading logging)
- Sandbox OU - Test/less rigid security
- Create other OU's and accounts
- Account factory -> create accounts in a automated way
  - Cloud formation is used to automate
- SCP's are used in Guard Rails

## Guard rails
- rules
- Mandatory, strongly recommended or elective
- Preventive
- Detective - Compliance checks

## Account Factory
- Automated Account provisioning
- guardrails, automatically added
- accounts can be closed or repurposed
- can be fully integrared with business SDLC.
