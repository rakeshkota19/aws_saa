
# Cloud Watch
- [Cloud Watch](#cloud-watch)
  - [Basics](#basics)
  - [Namespace](#namespace)
  - [Metric](#metric)
  - [Data point](#data-point)
  - [Dimension](#dimension)
  - [Alarms](#alarms)
  - [Create EC2 instance / Cloud watch](#create-ec2-instance--cloud-watch)

## Basics

- Collects and Manages operational data from other services
- Metrics - AWS products / Apps
- Cloud watch logs - Aws prods / apps
- Cloud watch events - AWS services & Schedules - generate events to consume them
- Public service .. usable for AWS or on-premises
- Store/Monitor/Access logging data
- AWS integrations - EC2/VPC flow logs/Lambda/Cloud Trail, R53
- Use unified cloud watch agent to integrate custom logs into Cloud watch

## Namespace
- Container for grouping
- AWS/service - namespace for aws service

## Metric
- collection of ordered set of data points.
- CPU usage / network in/out, disk io
  
## Data point
- CPU utilisation metric - each one is called datapoint.
- data point contains timestamp and value

## Dimension
- name/value pair to seperate data points. ( instance type / instance name)

## Alarms
- Alarms can be configured on the Metric. ( Notification can be sent using alarm)
- Three states ( OK / Alarm / Insufficient data)

## Create EC2 instance / Cloud watch
- create a ec2 instance and enable detailed cloud watch monitoring in advanced details. (Not covered in free tier)
- go to alarm in Cloud watch and create alarm
- select per-instance metrics(cpu utilisation) for the newly created ec2 instance. (identify using instance id)
- specify condition to go into alarm state
- configure action on what will happen, when alarm goes on
- install stress application to artificially create load on machine - used to test alarm