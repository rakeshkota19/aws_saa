
# Cloud trail

- [Cloud trail](#cloud-trail)
  - [Basics](#basics)
  - [Demo time](#demo-time)

## Basics

- Logs API calls/ account activities as a Cloud trail event
- Logs every change happening to a aws account ( action taken by user/resource)
- default stores 90 days, enabled by default
- retention period can be increased by creating a cloud trail
- 3 types of events
  - Management Events (control plane)
    - Management operations on resources (creating ec2/ deleting volume)
  - Data events
    - Operations being performed in resources
  - Insight events
- **by default**, only management events are logged (data events are very huge)
- Regional based service
- We can create single region trail or all regions trail
- Global services logs into the Us-east-1 service (North virgina-1). ( Global service events need to be turned on)
- For trails, Logs can be stored in S3, compressed json format. Only charged for the storage
- Not real time, typically delivers within 15 minutes of activity

## Demo time
