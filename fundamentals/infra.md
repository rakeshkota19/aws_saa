
# Infra
- [Infra](#infra)
  - [Regions](#regions)
  - [Availabilty Zones](#availabilty-zones)
  - [Edge Locations](#edge-locations)
  - [Resilience](#resilience)

## Regions
- Full deployment of AWS resources ( Full compute, Storage, DB, AI, Analytics)
- Spread throughtout the world
- Geographic Seperated/Isolated from each other, so fault tolerant
- Geopolitical seperation - different governance
- Location Control - performance
- Identified by region code

## Availabilty Zones
- Each Region has 2-6 AZ's.
- AZ's are isolated from each other - resiliency
- Connected through high speed redundancy

## Edge Locations

- only content distribution services and some edge computing
- have more locations than regions
- can be closer to the customer
- helps is faster transmissions
  
## Resilience

- Globally resilient - iam
- Region resilient - seperate service in each region
- AZ resilient - services runnning from single AZ