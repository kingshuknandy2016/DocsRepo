# AWS CONCEPTS

### What is microservice architecture ? <br/>
   
In microservice architecture, application is decomposed into small tiny applications/services which are developed and deployed independently.
These services are created to serve only specific business function such as user management, user roles, e-commence cart, product management, payments, order management, social media login etc.
They are independent of each other meaning they can be written in different programming languages, use different data storages and for different protocols.

### Amazon Autoscaling: <br/>
(Learn in Details theory and how to achieve it)

Scale up/down infrastructure as per the load for application.
Amazon Autoscaling service ensure that you have correct number of instances launch in Autoscaling Group to handle load for application. You can specify minimum and maximum number of the EC2 instances. Desired count is actual number of the instances required/running.

#### Types:
1.	Horizontal scaling : Horizontal scaling is adding more instances in pool of resources to handle load for application.
2.	Vertical scaling: Vertical scaling is to increase the capacity of existing hardware by adding resources like processing power, memory.

### Microservice architecute characterstics:
	Fault Tolerant: Fault tolerant system is responsible to deliver uninterrupted service, despite one or more components of infrastructure are failing. The purpose is to prevent catastrophic (complete, sudden, unexpected breakdown of application) failure of application.

	Secure : 	
	A Security Group acts as a virtual firewall that controls the traffic for one or more instances.
	A Network ACL is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.
	AWS WAF is a web application firewall that helps protect your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources.
	AWS Shield is a managed Distributed Denial of Service (DDoS) protection service that safeguards applications running on AWS.
	SSL Configurations

	High Availability
High availability refers to a system or component that is continuously operational for a desirably long length of time.
Availability can be measured relative to "100% operational" or "never failing“.

	Disaster Recovery
Disaster recovery is a set of policies and procedures which focus on protecting an organization from any significant effects in case of a negative event, which may include cyberattacks, natural disasters or building or hardware failures. Disaster recovery helps in designing strategies that can restore hardware, applications and data quickly for business continuity.

Recommendations:
-	Store instance AMI in multiple regions
-	Use S3 to store assets, and enable cross region replication
-	RDS – Enable cross regions replication
-	Dynamodb – Use global tables
-	Cloudformation Template


### ECS
Amazon Elastic Container Service (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage docker containers on a cluster. 
You can host cluster of container instances underlined running many services under docker containers.

#### Launch Type Models:
- Fargate <br/>
AWS Fargate is a compute engine that allows you to run containers without having to manage servers or clusters. With AWS Fargate, you no longer have to provision, configure, and scale clusters of virtual machines to run containers. AWS Fargate manages all the scaling and infrastructure needed to run your containers in a highly-available manner.
- EC2 <br/>
EC2 is cluster of ec2 instances. Customers who require greater control of their EC2 instances to support compliance and governance requirements or broader customization options can choose to this model.

#### Task defination
A task definition is required to run Docker containers in Amazon ECS. Some of the parameters you can specify in a task definition include:
* The Docker images to use with the containers in your task
* 	How much CPU and memory to use with each container
*	The launch type to use, which determines the infrastructure on which your tasks are hosted
*	Whether containers are linked together in a task
*	The Docker networking mode to use for the containers in your task
*	The ports from the container to map to the host container instance
*	Whether the task should continue to run if the container finishes or fails
*	The command the container should run when it is started
*	The environment variables that should be passed to the container when it starts
*	Any data volumes that should be used with the containers in the task
*	The IAM role that your tasks should use for permissions

#### Service:
You can run specific number of instances of task definition using service. If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it and maintain the desired count of tasks in the service depending on the scheduling strategy used. Load balancer is associated with service through target group and distributes traffic among multiple containers of service.

#### Rolling updates:
ECS performs rolling updates when you update an existing Amazon ECS service. A rolling update involves replacing the current running version of the container with the latest version. The number of containers Amazon ECS adds or removes from service during a rolling update is controlled by adjusting the minimum and maximum number of healthy tasks allowed during service deployments.
When you update your service’s task definition with the latest version of your container image, Amazon ECS automatically starts replacing the old version of your container with the latest version. During a deployment, Amazon ECS drains connections from the current running version and registers your new containers with the Application Load Balancer as they come online.

#### Service Discovery:
Service discovery is about to find another services running in network and consume it.
Challenge: if you are using cloud, your services may have dynamic network location due to restart, failure and scaling.
Amazon ECS now includes integrated service discovery. This makes it possible for an ECS service to automatically register itself with a predictable and friendly DNS name in Amazon Route 53. As your services scale up or down in response to load or container health, the Route 53 hosted zone is kept up to date, allowing other services to lookup where they need to make connections based on the state of each service.

#### ECR
Amazon Elastic Container Registry (ECR) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy docker container images. Customers can use the familiar Docker CLI to push, pull, and manage images. Amazon ECR provides a secure, scalable, and reliable registry.


## Small Project Setup
##### AWS RESOURCES:
VPC and underlined infrastructure<br/><br/>
*	VPC
    *	Internet Gateway
    *	2 Public subnets
    *	2 Private subnets
    *	2 Elastic IPs
    *	2 NAT gateways
    *	Route tables
    *	Network ACL
* Load Balancer
    *	Load Balancer Security Group
    *	Application Load Balancer

* ECS
    *	ECS container instance security group
    *	Bastion instance security group
    *	SNS Topic for infrastructure notification
    *	ECS container instance Role
    *	ECS container instance Profile
    *	ECS Cluster
    *	Default Target Group
    *	Launch Configuration
    *	Auto Scaling Group
    *	Scaling up policy and scale up Alarm
    *	Scaling down policy and scale down Alarm
    *	ECS Service autoscaling role
    *	ECS Service Role
    *	ECS Task Role
    *	Service discovery namespace
* Service
    *	Target group
    *	Target group listener rule
    *	Task definition
    *	Service discovery service
    *	ECS Service
    *	Service scalable target
    *	Service scaling up policy and scale up Alarm
    *	Service scaling down policy and scale down
* Alarm

==============================================================
Docker:
Docker is a tool to deploy and run applications by using containers.
Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. 
Unlike virtual machines, containers do not have the high overhead and hence enable more efficient usage of the underlying system and resources.
The container (Docker) becomes a convenient way to package up your application, including both the application layer and operating system dependencies. The final package keeps everything together that the application needs to successfully run, and it can be deployed to a production system as one complete unit.
Docker Commands:
Build
*	docker build -t myservice:1.0.0 -f Dockerfile .
Container
*	docker run --name name -it myservice:1.0.0 [command] –(command will override CMD in docker file)
*	docker run --name name -it myservice:1.0.0
*	docker run –name name -it myservice:1.0.0 /bin/sh
Login into Container
*	docker exec -it name [command]
*	docker exec -it name /bin/bash
AWS
*	aws ecr get-login --no-include-email
*	docker tag 30adb22e0974 582789473575.dkr.ecr.us-east-1.amazonaws.com/proteus/nodejs:helloworld
*	docker push 582789473575.dkr.ecr.us-east-1.amazonaws.com/proteus/nodejs:helloworld
List All Containers
*	docker ps -a
Delete all containers
*	docker rm $(docker ps -a -q)
Delete all images
*	docker rmi $(docker images -q)
===============================================
*	docker rmi $(docker images -q)

 
 
Aws has two services avaliable to deploy your microservices
ECS: Elastic Container service
EKS:  Elastic Kubernet Service


 
