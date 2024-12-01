## AWS

1. What is a region and availability zone?

<details>
  <summary>Answer</summary>

A region is a physical location where data centers are located. There are currently 14 regions available, each of which is completely isolated from the others and is a geographically united group of data centers. Each region has its own specifics both in terms of available services and prices.

A group of interconnected data centers is called an "availability zone." Each region consists of at least two availability zones, each of which is a separate data center with independent power sources and Internet cables.

About the addresses that your services receive. As a rule, they are all built according to the following scheme: <your name>.<unique identifier>.<region>.<service>.amazonaws.com. For example, a test RDS URL would be geopuzzle.cd1hw1nj0qdz.eu-west-1.rds.amazonaws.com. The full list of suffixes can be found at https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region.

</details>

2. What is the minimum set of services to run the infrastructure and deploy the application?

<details>
  <summary>Answer</summary>

These must be EC2 (virtual servers), S3 (object storage service), ELB/ALB (traffic balancing services), VPC (service for isolating your cloud in a separate virtual network), IAM (access control, users and rights), EBS (block storage), RDS (relational database service), Route 53 (DNS), Cloud Front (CDN).

</details>

3. What are the options for purchasing instances?

<details>
  <summary>Answer</summary>

1. On demand instances - usage-based pricing, the most popular.
2. Spot instances - unused resources in AWS, discounts up to 90%, can be suddenly turned off or taken away.
3. Reserved instances - cheap EC2 reserved instances.
4. Sheduled instances - allows you to reserve resources for a specific time.
5. Dedicated instances - instances with hardware-level isolation.

</details>


