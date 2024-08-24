
# ECS

## Intro

- Docker 
- Images .. set of layers
- Docker registry (tag/push/pull)

## ECS

- Managed container based compute service
- 2 modes
    - ec2
    - fargate

## Clusters

- Tell a way on how to run the docker images 
- ECR - AWS container registry
- Container definition - tells where image is, what ports it uses
- Task Definition - contains the definition for whole app - can contain multiple container definition inside it
- Task definitinon - stores the resources used by task/cpu/memory/networking/mode(ecs or fargate)/task role to access aws services
- Tasks can have one or more containers
- We can use ECS service to scale the containers also - copies, high availabilty, restarts

## Cluster type - EC2 mode

- ECS cluster runs inside a VPC, so they can run from multiple zones
- Not a serverless solution .. need to provide resoruces 
- uses container registory
- everything is managed by ec2 except, we need to manage the number of container hosts running

## Fargate

- we don't need to manage servers
- serverless
- containers are hosted on the shared fargate infrastructure
- they still operate in a VPC running through multiple AZ's
- only pay for containers running
- we dont need to manage containers/think about the provisions
