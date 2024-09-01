

# Serverless

## Event Architecture

- Monolithic - Fails together / scales together (vertical) / all are in single machine
  - Upload
  - Processing
  - Storage and Manage
- Tiered architecture - every tier is separated - horizontal scalable based on needs
  - Still has issue, one component needs other component to complete the work
  - Upload tier needs least one processing tier to process videos (youtube)
  - can be evolved using queues (decoupled)
  - upload tier will add to queue, the processing tiers will process the videos from queue
  - decouples the application
  - can be scaled independently
  - asynchronous
- Microservice architecture
  - multiple tiny applications interacting with each other
- Event producer -> Event router -> Event consumer
- Based on the events, resources will be scaled down and scaled up
- No constant running or waiting for things

## Lambda

- Faas - Function as a service
- functions are loaded and run from a runtime environment
- only billed for the running time
- short running and focussed
- supports multiple run times
- custom run time can be supported by layers
- docker, antipattern for lambda
- define the resources that will be used for lambda
- can run up to 15 minutes (900 s)
- permissions are given by execution role
- 2 networking modes
  - public
    - default, they can access public networks
    - best performance
    - can run on any shared networks
    - no access to VPC services unless they have public access
  - private
    - runs inside private vpc
    - can freely access resources inside vpc
    - cannot access public network unlike vpc is configured
    - old way has issue for every invocation need to create eni, but new way use common eni
- Security
  - needs a role to access any other services
  - execution role provides temporary creds
  - resource policy, who/what can invoke the lambda
  - lambda uses cloudwatch logs and X-ray (x-ray is used for distributed tracing)


## Lambda Invocation
- Synchronous
  - CLI/API invoke a lambda func, passing in data and wait for a response
  - lambda responds with the data
  - retries need to be handled with application
- Asynchronous
  - typically used when aws services invole lambda functions
  - if processing fails, lambda will retry b/w 0..2 times (configurable)
  - function needs to be idempotent
  - failed events can be sent to SQS/SNS
- Event source mapping
  - used on streams/queues/ dynamo streams/kinesis streams where events are not generated
  - kinesis stream, reads data from input but does not generate events
  - event source mapping, gets the source batch and sends this batch to lambda
  - batch size will change based on the job as the function time out is 15 minutes
  - lambda also needs permissions for event source mapping to interact with the event source
  - failed events will go to the SQS queues/sns topics

## Lambda - versions

- version - code + configuration
- immutable and has own arn
- $latest points to the latest version
- aliases will point at a version (can be changed)

## Lamda - run time

- invocated - execution run time needs to be created
- first resources need to be provisioned
- package needs to be downloaded
- cold start (full creation and configuration including function code download) - 100ms
- warm start - 1-2ms
- if any request comes in the same time, event is directly processed without cold start
- too longer period between invocations, old runtime will be discarded
- also one run time can run only one function, if multiple comes at a same time, multiple cold starts will happen
- by provisioned concurrency, aws will create N contexts warm, to improve speeds.

## Cloud watch events and event bridge

- Event bridge superset of cloud watch, they also handle events from third party
- If X happens at Y time, do Z 
- based on the events, we can perform any action
- Event bridge is Cloudwatch event v2 (*)
- A default Event bus for account
- cloud watch , only event bus (implicit) - not exposed
- event bridge, multiple event bus
- rules match incoming events (or schedules)
- routes the events to 1+ targets
- ex: ec2 is stopped, event is generated and add to event bus, which will be checked with rules, and matching rule will run


## Serverless Architecture

- manage few, if any servers - low overhead
- apps are a collection of small & specialised functions
- stateless and ephermeral environments - only duration billed
- event driven

## SNS - simple notification service

- highly available, pub/sub, durable
- coordinates the sending and delivery of messages
- public aws service
- messages <= 256 KB payloads
- sns topics are base entity - permissions and configuration
- publisher sends messages to topic and topics have subscribers which recv messages
- filter can be placed by the subscriber
- fan out -> send a message to all queues
- delivery status - (includng HTTP/Lambda, SQS)
- Delivery retries - Reliable delivery
- HA and Scalable (Region)
- SSE - server side encryption

## Step functions

- address limitations of lambda
  - lambda functions run time - 900sec (15 mins)
  - lambda - faas
  - they can be chained together for long running flow - but wrong approach
  - stateles
- state machines
  - workflow .. start point -> end point 
  - states are things which occur
  - maximum duration  1 year
  - standard workflow and express workflow 
  - started with API gateway, rules, event bridge, lambda
  - amazon states language (ASL) - Json Template
  - roles are used for permissions
  - states
    - succeed and fail 
    - wait state - will wait certain time/or till certain date
    - choice - different flow based on the input
    - parallel - parallel branches
    - map -  list of things, map performs an action on each of thing
    - task - single unit of work done (integrated with lot of aws services)
  
## Demo

- SES, create a email service
  - initial it will be in sandbox mode, so you need to whitelist the emails you want to send email
  - verify both emails
- Create a lambda function
  - create a execution role to interact with the SES
  - create a function to email remaind
- StateMachine
  - create iam role to allow statemachine to access aws services
  - use the ASL, json provided and created the state machine
  - update the arn of lambda
  - create it
- Create a Lambda function and API gateway
  - lambda - api gateway calls the lambda which invokes the statemachine
  - api gateway - rest api
    - regional
    - create resource/method and integrate with lambda
    - deploy to a new stage
- Create a bucket
  - enable all public access/policy
  - static hosting - (enable/ upload the files)

