
# Misc

## AWS - Glue

- Serverless ETL (extract/transform/load)
- uses the aws resource pool
- moves and transforms data between source and destination
- sourcce
    - s3/rds/jdbc compatible
    - streams (kinesis data stream/apache kafka)
- destination
    - s3, rds, jdbc databases
- Data pipeline also does etl but uses server

### Glue - Data catalog

- collection of metadata about data sources in region
- each for each region
- improves the visibility of data structure
- crawlers are configured to create data catalog, they determine schema, create meta data in data catalog

## Amazon MQ

- Merge b/w SQS and SNS
- SNS provides topics(1-many), sqs provide queues (1-1)
- queues are used for decoupling
- both are public services and highly scalable
- many orgs already use topics and queues
- to migrate their own into AWS, sns/sqs can't be used directly
- MQ is open source message broker - based on apache active mq
- provides both queue and topics
- Single instance or HA Pair
- not a public service, runs inside a vpc
- no aws native integration with other aws services
- only use when u need to migrate existing system/or need to protocols such as AMQP, MQTT, Open wire

## Amazon App flow

- full managed integration service
- exchange data b/w applications using flows
- sync data across applications
- aggregate from multiple sources

