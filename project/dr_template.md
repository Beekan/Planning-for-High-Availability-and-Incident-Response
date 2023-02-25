# Infrastructure

## AWS Zones
### Regions:

​		Primary: us-east-2

​		Secondary: us-west-1

### Availability Zones:

​		Primary: us-east-2a , us-east-2b

​		Secondary: us-west-1a , us-west-1c

## Servers and Clusters

### Table 1.1 Summary
| Asset                     | Purpose                                                      | Size                                                         | Qty                                                          | DR                                                           |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Asset name                | Brief description                                            | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |
| S3 Bucket                 | Object store to store the Terraform state                    |                                                              | 2                                                            | Created in two regions                                       |
| VPC                       | Virtual network to allow public or private communication.    |                                                              | 2                                                            | DR - One in each region of the two                           |
| Key pair                  | SSH key used to access EC2 instances                         |                                                              | 2                                                            | Created in multiple locations - One in each region           |
| EC2 instance              | Web server running code                                      | t3.micro                                                     | 6                                                            | DR - Created in two regions. each region has 3 servers       |
| Application Load Balancer | Becomes a front facing element that routes the requests to the ec2 instances and balancing load. |                                                              | 2                                                            | DR - one in each region.                                     |
| RDS cluster               | SQL database cluster with two nodes.                         | db.t2.small                                                  | 2                                                            | DR- Each cluster is deployed in a region and has one primary and one secondary nodes. |

### Descriptions
S3: Flat object store used to store data. Here it is used to store the terraform state which holds the current changes of the infrastructure.

VPC: For DR purposes subnets in each availability zone was utilized.

Key pair: SSH keys used to securely access the ec2 instances

EC2 Instances: Run the application on a virtualized machine.

Application load Balancer: Provides a first level DNS and balances the requests among the backend target group made of ec2 instances.

RDS cluster: Made of multiple nodes and hosts a SQL server that can hold the database of the application.

## DR Plan
### Pre-Steps:
- Copy ami image to another region.
- Create s3 buckets to store terraform state.
- Store or create a new ssh key to attach to the servers that are going to be created.
- Run the terraform script to create new VPC, EKS cluster, Load Balancer, RDS cluster.

## Steps:
You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region

- Stop the RDS cluster in the first region after checking that migration to the secondary region is fully complete and that the new RDS cluster gained primary privileges. 

- Change the DNS record in route53 that points to the load balancer in the first region to point to the new load balancer in the new region.