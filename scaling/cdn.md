
# CloudFront

## Intro

- Content delivery network
- improves the delivery of content to users
- uses caching
- farer the servers, longer the distance, higher the latency
- origion - source location of your content
    - s3 origin or content origin
- distribution
    - configuration unit of cloud front
- edge locations
    - local cache of your data
    - more distributed and there are edge locations in almost all major cities
    - we can't use them for deploying ec2
- regional edge cache
    - larger version of edge location
    - provides another layer of caching
- we can use ssl certificates with cloud front
- uploads go directly to the source, it is not write cache, it is read only cache

## Architecture

- Configure distribution
    - set origion, upload content
- distribution will be added to edge locations
- each distribution has a dns name
- first checked in edge location, then in the regional edge cache, and then it goes to the source. (Cache will be populated)
- origins - behaviour (patterns) - distribution

## Cloud front behaviours

- Requests coming into edge locations, are matched on a pattern
- default pattern is *
- matched with incoming request, 
    - it will be handled according to this options
    - origin policy
    - viewer protocol policy
    - allowed http methods
    - cache settings
    - min ttl/max ttl
    - restrict viewer access ( Trusted key groups/Trusted signer)
    - lambda

## TTL and invalidations

- more frequent cache hits = lower origin load
- Object stays in the cache as long it is valid
- 304 Not modified / 200 OK - when cloud front requests an object
- Validity
    - default ttl = 24 hrs validity period
    - we can set minimum and maximum ttl
    - define per object ttl values
    - origin header:
        - Cache-control max-age
        - cache-control s-maxage
        - expires (date & time)
    - both ttl & headers are respected
- Cache invalidation .. performed on a distribution
    - applies to all edge locations .. takes time
    - applies based on the patterns (/images/*, *)
- Versioned file names - A_v1.png, A_v2.png
    - logging is effective
    - no need to do invalidations
    - different from s3 versioning

## ACM - AWS Certificate Manager

- HTTP - simple and insecure
- HTTPS - SSL/TLS - data encrypted in transit
- ssl certificate provde identity
- ACM can be private or public CA
- Applications need to trust CA
- ACM lets you generate(acm renews them) or import certificates (our responsibility to renew them)
- supported aws services only (cloudfront/alb's not ec2)
- regional service
- load balancer needs to be in the same region as acm

## Cloud front / SSL

- Cloud front default domain name (CNAME)
- SSL supported by default
- we can add alternate domain names (CNAMES)
- for this global service, we need to add certificate in us-east-1 (northern virgina)
- 2 ssl connections : viewer -> cloudfront and cloud front -> origin
- both need valid public certificates (not self signed certificates)
- dedicated ip at each CF edge location to support non sni capable browsers (current 600 dollars per month)
- s3 origins handle certificates natively
- certificate should match the dns name

## Origin types / Architecture

- group of origin - origin groups
- s3 origin - directly we can use them

## Demo - CloudFront + S3 static hosting

- create a bucket, allow all public access and enable static hosting
- problem -> first of all users from far places have higher latency and also currently it is loaded using http not https
- Cloud front, go to distribution
    - use the bucket origin
    - path, you want to load data from
    - origin access
    - set the viewer protocol/cache policy/request policy
    - restrict viewer access - no
    - you can configure logging
    - Price class, where you want to cache
    - SSL certificate (for alternate domain, we need to add cname)
    - standard logging not real time
    - specify default root object - index.html

## Security

- Public Internet -> Edge locations -> regional edge cache -> source origin
- OAI - only applicable for S3 (origin access identity)
    - type of identity
    - it can be associated with cloud front distributions
    - cloudfront takes the identity of oai
    - oai can be used in bucket policies to deny all but one or more OAI's
    - edge locations pick up the oai identity
    - normal people will be restricted when they try to access s3
    - one OAI should be there for each distribution
- Custom origins
    - we can't use oai
    - custom headers to upgrade to https
    - custom origin requires custom header which will be sent by cloud front
    - no one see the content of https
    - in this way, the custom origins will allow the requests which have the custom header.
    - In another way, aws gives the ip's of all aws cloud front, and we use traditional firewall to restrict other ip's
- Public - Open access to objects
- Private distributions
    - requests require signed cookie or URL
- 1 behaviour - whole distribution can be public or private
- Old way: A cloudfront key created by root user, then account is added as a trusted signer
- new way: trust key group added
- Signed url's provide access to one object
- Use url's if your client doesn't support cookies.
- Cookies provide access to groups of objects
- use for group's of files
- Origin access control settings - edit distribution/create/sign the requests/update the bucket policy
    - no one can access the s3 origin except through the cloud front
    - cloud front is signs every requests with the distribution and the s3 checks the distribution and validates will the help of bucket policy

## Lambda @Edge

- allows to run lightweight lambda on the edge locations
- only nodejs/python run time and no support of layers
- run in aws public zone
- viewer req -> lambda run -> origin request -> origin response -> viewer response
- lambda - viewer side (128MB/5seconds)
- lambda - origin side (30 seconds/ normal memory usage)
- common use cases:
    - A/B testing - viewer request
    - Migration between s3 origins - origin request
    - different objects based on device - origin request
    - content by country - origin request

## Global accelerator

- problem: 
    - people from very far distance, latency will increase, as the number of routers will increase / more hops will be done
- 2 * anycast ip address 
    - multiple devices can use this ip, and can be in multipe locations
    - 1.2.3.4 && 4.3.2.1
- Traffic initially uses public internet and enters a global accelerator edge location
- From the edge, data transits globally across AWS global network, less hops, significant better performace
- moves the aws network closer to customers
- network product (non https/tcp/udp)
