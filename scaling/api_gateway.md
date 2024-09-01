
# API Gateway

## Intro

- Create and manage API's
- endpoint/entry point for applications
- sits between apps and integrations
- ha, scalable, handles auth, throttling, caching
- can connect to services/endpoints in AWS or on-premises
- HTTP API's / Rest API/ Web socket
- request - auth / validates / transforms (such a way that services can consume them)
- response - transforms/ prepare/ return
- cloud watch logs

## End point types

- edge optimised - routed to the nearest cloud front pop
- regional - clients in same region
- private - only accessed in vpc

## Stages

- api are deployed to stages (dev/test/prod)
- each stage has one deployment

## Errors

- 4XX - client error (permmissions/headers issue)
    - 400 - bad req
    - 403 - access denied - auth issue
    - 429 - throttled
- 5xx - server error (valid req/backend issue)
    - 502 - bad gateway exception - bad output returned
    - 503 - service unavailable
    - 504 - Integration failure/timeout - 29s limit

## Caching

- Cache defined per stage
- ttl is 300 seconds, configurable from 0 to 60 minutes
- can be encrypted
- cache size - 500MB to 237 GB
