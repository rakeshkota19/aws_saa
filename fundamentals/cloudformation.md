
# Cloud Formation

- [Cloud Formation](#cloud-formation)
  - [Basics](#basics)
  - [Template](#template)
  - [Template to create EC2 instance](#template-to-create-ec2-instance)
  - [Automation](#automation)

## Basics

1. Used to automate d2d activites using template.
2. Template can be written in yaml or json

## Template

1. Resources - list of resoruces on which actions are performed - mandatory
2. Description - string describing ( Must always follow aws template format version)
3. Metadata - control how things are presented in console cloud formation UI
4. Parameters - create fields/values 
5. Mappings - help to create look up tables 
6. Conditions - help to create conditions before running script
7. Outputs - once the template is run, it tells how to give output

## Template to create EC2 instance

1. Create a Template and run
2. It will create a stack ( virtual resources ) .. Stacks can be run multiple times.
3. For every resource, a physical resource will be made similar to logical resources
4. Stacks can be updated/deleted.

## Automation

1. Go to cloud formation
2. Create a new stack
3. Select the ec2instance template and it will be uplaoded to s3 with cf prefix bucket
4. Leave all the options to default.
5. Create stack ( First logical resources will be created and then physical will be created)
6. Delete stack ( First deletes logical resources then physical will be removed) 
