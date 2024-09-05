
# Security

## AWS Secrets Manager

- parameter store vs secrets manager
- it shares func with parameter store
- designed for secrets (passwords, API keys)
- usable via console, cli, api or sdk's
- supports automatic rotation .. this uses lambda (only supports for some products)
- directly integrates with some aws products ( .. RDS)

## Application L7 firewall

- Normal firewalls
    - L3 - request/repsonse are different and unrelated
    - L4 - request/repsonse are different and unrelated
    - L5 - with session capability - request/repsonse are one - less over head
    - all three can't see the data
- L7 firewall
    - can identify normal or abnormal requests specific attacks
    - firewall will terminate https, so it sees all http content and can block malware request
    - able to block certain applications


## Web application firewall (WAF)
- AWS implementation of L7 firewall
- Components
- Web ACL 
    - Rules within rule groups controls how waf reacts to incoming traffic
    - can have deny list / allow list / sql injection / xss
    - default action - allow or block - non matching
    - resource type - cloud front or regional service (alb, api gw, app sync)
    - rule group - processed in order
        - need compute requirement - Web ACL capacity units (WCU) - default 1500
        - web ACL's are associated with resources (this can take time) - adjusting a web acl takes less time
        - contains rules
        - managed by us or service owned
        - have a wcu capacity (defined upfront, max 1500)
        - rule: type (regular/ rate), statement (what to match,  count all, what/count), action (deny/allow, count, capcha, custom response)
        - matchable elems: IP, label, header, cookies, query params, uri path, body (first 8KB only), HTTP method
        - rate based rules, you only have block/capcha
        - multi stage flows, labels can be referenced in the later requests
- Output logs to S3/CW logs/ Firehose
    - S3, it takes 5 mins, so should not be used
- pricing -
    - web acl - monthly (5 dollar per month)
    - rule on web acl - monthly ($1 per month)
    - requests per web acl = monthly (0.60 dollar per month)
- Intelligent threat mitigation
    - Bot control
    - Captcha
    - Fraud control / account takeover
    - Market place rule groups


## AWS shield

- 2 forms - Shield standard and advanced
- standard free, advanced extra cost
- attacks 
    - network volumetric attacks (L3) - saturate capacity
    - network protocol attacks (L4) - TCP SYn flood, leaves connections open, thus preventing new ones
    - application layer attacks (L7) - web request floods
- Standard
    - free for aws customers
    - protection at region/vpc or edge
    - blocks L3/ L4 attacks
- Advanced
    - $3,000 per month (per ORG), 1 year lock in + data out/,
    - protects cf, r53, global acc, alb's, clb, nlb
    - not automatic, must be explicitly enabled in shield advanced or fire wall manager
    - cost protection (e.g ec2 scaling for unmitigated attacks)
    - proactive engagement and AWS shield response team (srt)
    - WAF integration, protects from L7 attacks
    - real time visibility of DDOs events/attacks
    - health based detection
    - protection groups

## Cloud HSM

- KMS
    - Key management service
    - gen keys, all services integrate with KMS
    - KMS is shared but seperated
    - aws has access to the product 
    - aws runs kms on the hsm (harware security model)
    - all operations are done by amazon api's
- We can run our own HSM
    - aws provisioned, fully customer managed
    - fully fips 140-2 level 3 ( kms is L2 overall)
    - this is accessed by Industry standard api's ( PKCS11, JCE, Microsoft crytpoNG (CNG) libraries)
    - KMS's uses cloud HSM as a custom key store, integration with KMS
- deployed into Cloud HSM VPC
    - by default not a HA, runs in a single AZ
    - run multiple, and keep them in sync
    - there are injected into our vpc through eni
- AWS provision HSM but they have no access to secure area where material is held
- No active integration with AWS Services (eg: no s3 sse)
- offload the ssl/tls processing for web servers (kms cannot but hsm can do)
- enable transparent data encryption (TDE) for oracle databases
- protect the private key for a ssl certificate

## Aws config

- 2 main jobs
- record configuration changes over time on the resoures
- auditing of changes, compliance with standards
- does not prevent changes happening .. no protection
- regional service .. supports cross-region and account aggregation
- changes can generate sns notifications and near realtime events via event bridge
- changes are stored in config bucket
- Optinal features:
    - Config rules evaluate resources against a standard to check if there are compliant or not
    - AWS can update using SNS/SES to update the customer regarding the issue and they can do the remediation

## Amazon Macie

- Data security and privacy service
- discover, monitor and protect data stored in s3 buckets
- automated discovery of data, PII, PHI, Finance
- managed data identifiers - built in ML/Patterns
- custom data identifiers - proprietary - regex based
- run jobs with the required identifiers
- integerates with security hub and finding events to event bridge
- centrally manage .. either via org or inviting others
- Discovery job, has a discovery schedule + identifiers -> findings are generated can be integrated with other aws services.
- managed data identifiers - maintained by aws, we can also create custom identifiers
- types of findings
    - policy findings
        - policies/ security settings are changed which reduces the security of bucket
    - sensitive data findings
        - credentials, custom identifier, financial, multiple, personal

## Amazon Inspector

- Scans the Ec2 instances and the instances OS/containers
- vulnerabilities and deviations against industry standard
- length .. 15 min/1hour/8/12/24
- findings report ordered by priority
- two assessments
    - network assessment (agentless) - network reachability
    - network and host assessment (agent) - checks against cve / cis benchmarks / security best practises for amazon inspector

## Amazon Guard duty

- Continous security monitoring service
- ai/ml + threat intelligence feeds, identifies unexpected and unauthorised activity
- learns patterns of what happens normally
- analyses supported data sources
- notify or event driven protection/remedidation
- supports multiple accs (master/member)
- DNS logs/ VPC flow logs/ cloud trail logs/ CT management events / ct s3 data events -> guard duty checks and find the issues





