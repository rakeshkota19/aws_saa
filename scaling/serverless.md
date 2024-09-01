
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
  - 
