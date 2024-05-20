
# Cloud trail

- [Cloud trail](#cloud-trail)
  - [Basics](#basics)
  - [Demo time - Organisation Trail](#demo-time---organisation-trail)

## Basics

- Logs API calls/ account activities as a Cloud trail event
- Logs every change happening to a aws account ( action taken by user/resource)
- default stores 90 days, enabled by default
- retention period can be increased by creating a cloud trail
- 3 types of events
  - Management Events (control plane)
    - Management operations on resources (creating ec2/ deleting volume)
  - Data events
    - Operations/actions being performed in resources
  - Insight events
    - unusual activity/error/user behaviour in account
- **by default**, only management events are logged (data events are very huge)
- Regional based service
- We can create single region trail or all regions trail
- Global services logs into the Us-east-1 service (North virgina-1). ( Global service events need to be turned on)
- For trails, Logs can be stored in S3, compressed json format. Only charged for the storage
- Not real time, typically delivers within 15 minutes of activity
- one trail is free per account

## Demo time - Organisation Trail

- Go to Organisation trails, create a new trail
- Default, a trail will be created for all regions.
- For management user, you can also select single trail for all accounts
- Storage location, create bucket or use existing bucket
- Log file validation - extra layer of security
- Option to add them to Cloud watch logs, can monitor them using cloud watch and create events based on that
- We need to provide a role to cloudtrail so that it can access cloud watch (Create new role)
- Ingore Data/Insight Events
- Create trail
- First set of logs delivery to S3 can take up to 15 minutes.
- Event history stores logs of all cloud trail events, even for the inactive trails.
