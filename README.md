# create-vpc
AWS VPC Creation
Customer currently using Microsoft Azure and their existing environment located in Azure Datacenter. They are using Azure WepApps, CosmosDB and Azure SQL Database services on Azure. They’ve created Azure App service with Node.js platform.

This project aims to migrate customer's applications from Azure to AWS without interruption and smoothly.

![Spirohome (4)](https://user-images.githubusercontent.com/68296051/94397315-ddd44c00-0163-11eb-815f-df8956921889.jpg)


Customer application will be designed in a two-tier architecture pattern. Application logic is implemented in an EC2 server managed by AWS Elastic Beanstalk and Data tier is implemented in RDS and AWS Managed DocumentDB. Both tiers are scalable. For infrastructure administration and maintenance, a Bastion host is deployed in each public subnet for 3 AZ. It is a highly secured and created from a prebuilt AMI provided by AWS. It will allow ssh connection only from a trusted IP source. Application servers and Database servers are hosted in private subnets. It can be only accessed from the Bastion host. Servers can be connected only by key pair authentication to avoid vulnerabilities. App server can access the internet through NAT gateway for software installation.

Elastic load balancer is a user-facing component to accept the web requests in Customer application. This traffic is routed to the backend EC2 servers implemented by Beanstalk service. Backend server takes care of processing the web request and return the response to ELB which is then consumed by the end-user. ELB is deployed in a public subnet and it is secured by a VPC security group which will allow only http/https inbound traffic from external sources. ELB will only access the back end servers either by http/https protocol. To ensure high availability and uniform distribution of traffic, we have enabled cross-zone load balancing. Apart from that, we have configured the load balancer to support session persistence, maintaining idle timeout between the load balancer and the client.

We will use RDS SQL and DocumentDB database as a database tier for the application. It is deployed as a cluster with read/write endpoints. Both servers and database instances are secured by strong security group policy to avoid access from an untrusted network source.

AWS Code commit is a source code repository for this project, It is a highly available, private repository managed by AWS. S3 bucket is used for storing the artifusted network source.

AWS Code commit is also a source code repository for used infrastructure resources. Customer infrastructure will be created and managed by AWS Cloudformation. This CFN template will created by Commencis Cloud Team.

AWS Code commit is a source code repository for acts. This artifact is used by AWS codepipeline to deploy it on different environment.

CI/CD pipeline is the core component of this project which builds the code and deploy the changes to the server. We use AWS Codepipeline for building a CI/CD pipeline.

AWS Elastic Beanstalk for Node.js makes it easy to deploy, manage, and scale your Node.js web applications using Amazon Web Service. We decided to use AWS Beanstalk service developing web application using Node.js. After deploy Elastic Beanstalk application, Customer can continue to use EB CLI to manage their application and environment, or they can use the Elastic Beanstalk console, AWS CLI, or the APIs.

Using DMS, we’ll perform live migrations to Amazon DocumentDB from MongoDB replica sets, sharded clusters, and from Azure Microsoft SQL Server databases to AWS RDS MsSQL with minimal downtime. Before starting database migration Site2Site VPN will be implemented between AWS and Azure.
AWS WAF is a web application firewall that helps protect Customer web applications against common web exploits that may affect availability, compromise security, or consume excessive resources.
