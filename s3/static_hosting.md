
# Static Hosting

## Intro

- Normal to access S3, we use AWS API
- S3 can also be accessed using HTTP
- Index/Error documents need to be set -> Website endpoint is created
- Custom domain with a website, only if it matches with bucket names


## Offloading vs out-of-band pages

- Out of band -> Backup site hosted on s3.
- Offloading -> Client -> S3 -> html returned -> statuc images loaded from s3

## Demo

- S3, create a new bucket ( for custom domain, use same name for bucket)
- Uncheck block public access / Create a bucket
- Bucket / Properies / Static Hosting
- Enable / Specify Home and Error page / Save changes
- Upload objects to S3 for use in static hosting
- try to access the site / 403 forbidden - as s3 is private by default
- grant permissions so that anyone can view
- permissions tab / edit bukcet policy and update resource arn
- Now, we should be able to view the page
- Customise domain, go to route 53, hosted zones and go to the zone and define a record
- Record - record-name, record-type:a, update traffic route information
  