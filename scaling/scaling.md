
# Scaling

## Intro

- 3 types of architectures
    - small scale, exist in one region
    - dr, exist in one region, and stand over in one region for dr
    - multiple regions application
- global dns used for service disovery and regional based health checks, request routing
- CDN's are used to cache content globally - as close to end users ( Cloud front)
- Initial entry point (web tier) - load balancer or API gateway
- computer tier - ec2/ lambda / containers
- storage tier - ebs/efs/s3
- caching - elasticache
- DB tier - RDS/Aurora/Redshift/Dynamodb
- App services - sqs/sns/kinesis functions

## ELB

- 3 types
    - classic load balancer (CLB) - v1 - 2009
        - http and https requests
        - lack advacned features
        - only one ssl certificate
    - ALB - v2 - HTTPS/Websocket
        - preferred
    - NLB - network load balancer - v2
        - supports tcp/tls/udp
- v1 (avoid/migrate) and v2 (prefer)
- v2 - faster/cheaper/support targer groups and rules
- when creating ELB
    - pick ip4 or ip4/ipv6
    - pick AZ's/subnet groups
    - elb, has a A record dns name pointing to the ELB nodes (1+ nodes per AZ)
    - internal (only have private ips) or internet facing (have public ips)
    - listener config - which port/prototol to listen on and communicate
- need 8+ free IP's per subnet and /27 or larger subnet to allow for scale (/28 can also be sufficient)
- ec2 doesn't need to be public to work with a LB

## Cross zone LB

- Web -> LB DNS Name -> Hits the LB -> either of the node's in different AZ are hit
- scenario 2:
    - Main LB -> Seperate LB in Each AZ -> 
    - Under zone LB's, they have different number of servers (for ex: 4 and 1)
    - the main LB sends equally to both the zone LB's but as the server distribution is uneven, one server on the right will handle 50 percent of the requests
    - Sol: Cross zone LB
        - Allows distribute eqaully, i.e LB in one zone can distribute to nodes in other zones as well

## ALB vs NLB

- for CLB's, every single unqiue https requires a single CLB
- NLB, ALB can work on multiple http calls based on the listener config
- ALB
    - L7 LB, listens on HTTP/s
    - can not understand other L7 protocols SMTP,SSH,Gaming
    - no UDP,TCP listeners
    - L7 content type, cookies, custom headers, user location, app behaviour (NLB can't do this)
    - SSL terminated at LB, and new connection is made to instance from LB
    - slower than NLB, as they need to process more network layers
    - as there are L7, they can also perform health checks to check
    - Rules:
        - based on the rules, they are routed at listener config
        - processed in priority order
        - default rule - catchall
        - rules can have conditions
        - conditions can be based on host-header, http-header, http-request-method, path-pattern, query-string/source-ip
        - actions can be forward, redirect, fixed-response, authenticate
        - based on the condtions, actions will be done and sent to the target groups
    - if you dont want to terminate ssl connections at load balancer, you need to use NLB
    - supports slow start mode
- NLB
    - L4, only intrepret TCP, TLS, UDP
    - No visibility or understanding of HTTP/s
    - No headers/cookies/session stickiness
    - very very fast, millions of rps, 25% of ALB latency
    - can be used for SMTP, SSH, Game servers, fin apps
    - can't do detailed health checking
    - they just check ICMP/TCP handshake
    - forward TCP to instances, so unbroken encryption
    - used with private link to provide services to other VPC's
- ALB vs NLB
    - pick NLB for
        - unbroken encryption
        - static ip for whitelisting
        - fastest performance
        - not http/https protocol
        - private link

## Launch Config's and launch templates

- allows us to define EC2 instance in advance (AMI, instance type, storage / key pair/networking/security groups/userdata/iam role)
- both are not editable, once created. Launch templates can have versions
- Templates are newer versions and also have T2/T3 unlimited, placement groups, capacity reservations, elastic graphics
- Templates are preferred
- used in Auto scaling group, these instances will be launched by ASG
- Launch config can also be used in ASG
- Launch templates can be used in ASG as well as console UI/CLI


## Auto scaling groups

- automatic scaling / self healing for ec2
- uses launch config or templates for spinning up the instance
- has minimum,desired and maximum size (x:y:z)
- generally desired capacity instances are provisioned by the asg
- scaling policies automate based on metrics
    - manual scaling - manually adjust the desired capacity
    - scheduled scaling - time based adjustment (sales)
    - dynamic scaling
        - simple - rule (CPU > 50% +1 else -1, based on many metrics)
        - stepped scaling - bigger +/- based on difference.. preffered over simple
        - target tracking - desired aggregate cpu = 40$ defined and the ASG handle it
- ASG .. launch template provides ec2 config
- Cool down periods (how much time, must asg wait from the the previous action)
- health of instances (ec2 status checks), asg will terminate the instance and replcae it with new instance
- are free
- use cool down period to reduce costs/rapid scaling
- think about more, smaller instances - granularity
- asg defines when and where, lt defines what

## ASG + Load Balancers

- ASG is integrated to a target group
    - asg adds/removes instances to target grp 
    - asg can use load balancer health checks rather than ec2 status checks 

## Scaling processes

- Launch and Terminate - Suspend and Resume
- AddToLoadBalancer
- AlarmNotification
- AZRebalance
- HealthCHeck
- ReplaceUnhealthy
- ScheduledActions
- Standby

## Scaling policies

- Manual - manually changes min,max, desired - used for testing/urgent
- Dynamic
    - Simple scaling: 
        - simple conditons based on the metrics ( cpu > 50 % add 2 instances, cpu < 50 % remove 2 instnaces)
        - inflexible
    - Step scaling:
        - define upper and lower bounds (50-60, do nothing, 60-70, add one instance, 70-80, add one more)
        - better than simple scaling
        - multiple steps instead of single conditon on a metric
        - always the number of instances will be between min, max
    - Target tracking
        - predefined set of metrics
        - 50% cpu on overage, based on this metric, auto scaling adds/removes the instances
    - scaling based on SQS - Approximate number of messages visible
- For scale in, which node will be picked

## Life cycle hooks

- custom actions on instances during asg actions
- instances are paused within the flow .. they wait
    - until a timeout
    - or you resume the ASG process
- event bridge or sns notifications
- Lifecycle: Pending -> Inservice -> Terminate
- hooks can be added at pending or terminate period 
- these can be done to perform some action such as backing up logs / clean up
- they can be integrated to sns / event bridge to initiate other processes

## Health check comparsion - Ec2 vs ELB
- 3 types
    - ec2 (default) - based on instance 2/2 status checks
    - elb checks - running and passing elb health check (can be made application aware)
    - Custom - Instance marked healthy and unhealthy by an external system
- Health check grace period
    - period before health check is ran when new instance is setup (system launch/bootstrapping/application start)
    - having small grace period can make the asg repeatedly start and stop the new instance

## SSL offload and session stickiness

- 3 ways, encryption is handled by load balancer
    - Bridging 
        - default
        - ssl is terminated at LB, so LB needs the ssl certificate
        - then ELB initiates a new ssl connection to backend instances, need ssl and compute power for crypto-operations 
        - overhead is signifant for high volume requests
        - risk, certifcate should be in load balancer and also extra operations at ec2
    - Pass-through
        - client -> lb -> server (directly passed through)
        - mostly NLB
        - LB doesn't touch the encryption
        - we can't do load balancing based on HTTP
    - Offload
        - ssl is terminated at LB
        - LB creates a new HTTP request to the instance
        - so ec2 has no extra crypto operations
        - data is in plain text form inside the AWS environment

- Connection Stickiness
    - no stickiness/stateless - we can send any request to any box (user need to handle user state)
    - aws 
        - when a user comes first time to LB, it creates a cookie (AWSALB), and from next time, when cookie is received, request will be sent to the same server
        - this will happen until cookie is expired or if the server is taken down / health checks are failed
        - unequal distribution
    
## Architecture Demo

