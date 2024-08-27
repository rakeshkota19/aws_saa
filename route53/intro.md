
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

## Simple routing

- supports one record per name (A record, www -> multiple ip's)
- doesn't support health checks

## R53 health checks

- health checks are seperate from, but are used by records
- performed by health checks located globally
- occurs every 30s (every 10s costs extra)
- TCP, HTTP/HTTPS, HTTP/HTTPS with string matching
- based on checks, it will be assessed healthy or unhealthy
- 3 types - endpoint, cloud watch alarm, check of checks

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

