
# Queues

## SQS

- Messaged Queues
- Public
- HA Queues - Standard (best effort order) or FIFO (order guarnteed)
- messages <= 256KKB in size
- Received messages are hidden (VisibiityTimeout) - either reappear or explicitly deleted
- Dead letter queues - can be used for problematic messages
- ASG's can scale and Lambdas can be invoked based on queue length
- SNS + SQS can be used for more use cases
- Billed based on requests
- 1 request - 1-10 messages up to 64KB total
- Short (intermediate) vs long(waitTimeseconds - upto 20 sec) Polling
- short polling - more requests(even data can be empty) - more price
- encryption at rest(kms) & in-transit

## types

- Standard
    - at-lease once delivery (sometimes they can be delivered than once occassionally)
    - multi-lane-highway
    - more performance with fifo
- FIFO
    - exactly-once processed and order guarnetted
    - single lane highway
    - less performance (3000 per second with batching or up to 300 msgs)
    - they should have fifo_suffix
    - used where there is need for processing in order

## Delay queues

- Visibility timeout
    - When some one receives the msg from queue, visibility time out starts and the msg is not visilbe to any consumers. If the processing is failed, and it is not deleted by the first consumer, then it will reappear in the queue. (default - 30s, 0 < 12hrs) - set on queue or per-message
    - used for automatic re-processing
- Delay queue
    - delay seconds
    - messages added to queue will be invisible for the delay seconds
    - min (0) and max (15 minutes)
    - per-message setting is not allowed in fifo queue

## Dead letter queue

- all problematic messages are added in the dead letter queue
- based on the maxReceiveCount, it will be moved to dead letter queue
- retention period of dead letter queue is longer than source queues



