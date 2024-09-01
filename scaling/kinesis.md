
# Kinesis

## Kinesis - Data Streams

- Scalable streaming service
- real time
- producers send data into a kinesis stream
- streams - low to near infinite data rates
- public service and ha by design
- streams store a 24 hour moving window of data (max: 365 days)
- multiple consumers access data from that moving window
- shards are added based on the data (more shards more price)
- big window - big shards
- kinesis data record - 1MB
- SQS vs Kinesis
    - Ingestion or decoupling
    - SQS 1 production group/1 consumption group
    - SQS, no persistance of messages, no window
    - kinesis - huge scale ingestion/multiple consumers/rolling window
    - kinesis - data ingestion, analytics, monitoring, app clicks

## Kinesis - Data Firehose

- Fully managed service to load data for data lakes, data stores and analytics
- used to persist the data which becomes out of the window of data stream
- automatic scaling .. fully serverless .. resilient
- near real time delivery (~60 sec/buffer fills in 1MB)
- billing - volume through firehose
- supports transformation of data on fly (lambda)
- deliver data to
    - http end point
    - splunk
    - redshift (uses s3 as an intermediate)
    - elastic search
    - s3 bucket

## Kinesis - Data analytics

- real time processing of data using sql
- ingests data from data streams or fire hose or s3 data
- destinations to data stream / firehose + s3/redshift/elastic search/splunk
