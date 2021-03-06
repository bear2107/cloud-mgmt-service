# Design Requirements
## Async
As a user, I want long running tasks to be handled asynchronously, so that cloud resources have enough time to be created, because creating cloud resources could take a long time and clients will timeout waiting for a synchronous response. 

## REST Interface
As a user, I want a cloud agnostic REST interface, so that provider resources are not directly exposed, because being able to keep my client implementation simple will allow me to more effectively scale.

## Infrastructure as Code
As a user, I want Terraform to be used for creating and deleting cloud resources, so that my infrastructure can be managed as code, because if my infrastructure is not tracked it cannot be easily managed. 

As a user, I want Terraform state to be persisted on durable network storage, so that future updates to my cloud resources can be performed, because if Terraform state is lost I will be unable to update my cloud resources. 

## Infrastructure Monitoring
As a user, I want my cloud resources to be monitored, so that configuration drift and future changes can be managed, because resources untracked resources will be more difficult to update. 

## Security
As a user, I want token authentication and authorization implemented at runtime, so that REST resources are protected, because without security cloud resources may be manipulated by unauthorized users. 

# Decisions Made During Requirements Gathering
The following decisions were made during requirements gathering:

##  Terraform State Location
In order to move fast, Terraform state should be stored in a local filesystem backed by EBS. This will allow the Crate team to leverage previous work. In the future, a remote solution that allows for locking, such as S3, should be used. 

## Async APIs
Due to the long running nature of infrastructure provisioning requests, some APIs will need to be asynchronous. This will allow clients to request resources and eventually receive them, preventing timeout issues. 

## Message Queues
Message queues should be available for implementation following MVP. REST API interfaces should be designed with this in mind. 
