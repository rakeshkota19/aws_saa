
# Intro

## hosted zones

- public / private
- globally resillent service
- dns db for a domain
- automatically created with domain registration via R53
- hosts dns records (A,AAAA,MX,NS,TXT)
- name server records are entered at tld when your register a website

## Public hosted zones

- DNS database hosted by R53, publically name servers
- accessible from public internet and VPC's
- hosted on 4 name services for the zone
- use ns records to points at these NS
- Resource records(www, mx, txt) created within hosted zone
- within vpc, resolving happens directly in r53
- outside the internet, resolving happens from root servers(tld/isp servers)

## Private hosted zone

- similar to public, but its private
- only accessible through associated vpc
- split view for public and internal use with the same zone name (overlapping public and private)


## R53 health checks

- health checks are seperate from, but are used by records
- performed by health checks located globally
- occurs every 30s (every 10s costs extra)
- TCP, HTTP/HTTPS, HTTP/HTTPS with string matching
- based on checks, it will be assessed healthy or unhealthy
- 3 types - endpoint, cloud watch alarm, check of checks (health check of other checks)
- endpoint check 
    - can be based on ip/domain
    - checks are generally per 30 sec, however we can pay for 10sec
    - failure threshold of 3
    - can also add string matching
    - health checker regions (default or customised)
    - we can create an alarm to notify us on failed health checks (SNS topic)
- fail over routing
    - primary / secondary
    - if primary fails, it will be routed to secondary
    - out of band like using s3 static hosting
    - routing happens based on health checks
    - active passive failover
- demo
    - you should have a domain
    - create the necessary infra - ec2/vpc
    - give a elastic ip to the instnace - static
    - create a bucket with domain name to host static website
    - enable static hosting, upload relevant files, and upload policy
    - create a health check, endpoint health check for the earlier ec2 instance's ip
    - create record in the hosted zone - wizard mode
    - fail over record - www (create and update all relevant properites) 
    - create two fail over records, one for primary and secondary (also give health check in the fail over record)
    - fail over routing works now

## Routing

-  Simple routing
    - supports one record per name (A record, www -> multiple ip's)
    - doesn't support health checks
- Multi value routing
    - mixture of both simple and failover routing
    - it supports multiple reccords with the same name
    - upto 8 healthy records are sent (only healthy are sent)
    - improves availability
- Weighted routing
    - Used when you want to test new version of softwares
    - weight for each record, and based on weight, dns returns the ip accordingly
    - 0 weight is never returned
    - used for simple load balancing
- Latency routing
    - optimise performance and improve latency
    - for each record ip/name we can specify record region, where the instance is
    - so it will route to the instance which has smaller latency
    - AWS mantains database of latnecy between the user's locations and regions
- Geolocation routing
    - each record is tagged with location (country/continent/ default/state)
    - it gets the user location from IP
    - doesn't return closest records, but only relevant records are sent
    - first checks state/country/contintent and then default if not found in any order
- Geoproximity routing
    - returns the closest ip to the user based on the distance
    - we can add bias to the instances region to surround near by areas so that routing can happen to that region

## Interoperability

- R53 has 2 jobs when you register a website
    - domain registrar
    - domain hosting
- it allocates 4 name servers and creates a zone file on above NS
- then it communicates with the TLD registry

## Misc

- cname / alias
    - DNS, A maps a name to ip address
    - CNAME maps a name to another name (www.catagram.io => catagram.io)
    - cname is invalid for naked/apex (catagram.io)
    - Many AWS services use a DNS name (ELB's)
    - with just CNAME, catagram.io -> ELB is unsupported
    - Alias - map a name to aws resource
    - can be used for naked/apex and normal records
    - alias is encouraged by AWS (default)
    - a record alias / b record alias
    - api gateway, cloud front, ELB, S3 - A record
