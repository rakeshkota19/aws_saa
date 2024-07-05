

# Optimisations
- [Optimisations](#optimisations)
  - [Intro](#intro)
  - [Issue - Single stream](#issue---single-stream)
  - [Sol - Multi-part upload](#sol---multi-part-upload)
  - [S3 Transfer acceleration](#s3-transfer-acceleration)
  - [Demo](#demo)

## Intro

## Issue - Single stream

- Single data stream to S3
- If upload fails, we need to restart again
- Speed and reliabilty -> limited by single stream

## Sol - Multi-part upload

- Data is broken up and and sent ( Minimum is 100 MB)
- max of 10,000  max parts, ( 5mb -> 5gb, last part can be less than 5 mb)
- Parts can fail, and be restarted
- parallel uplaod, transfer rate increases
  

## S3 Transfer acceleration

- Current s3 requests orignating comes from the outer network and they can unreliable.
- Public network cannot always give best performace, and sometimes be unreliable
- Transfer Acceleration, uses the aws edge locations to connect to s3
- by default it is turned off, and it requires the name to be dns compatible, no commas in the name
- Clients finds nearest closes edge first and transit to the region, it needs to talk to
- AWS region is perfectly linked to other regions/edge locations and can give better performance.
- AWS network is like express train
- farther the distances, more are the benefits of accelerated transfer.

## Demo

- Create a bucket
- Properties, enable transfer acceleration, copy the accelerated endpoint
- Comparison tool - https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html