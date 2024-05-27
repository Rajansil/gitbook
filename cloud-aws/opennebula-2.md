# 🌥️ VPC

It is a Amazon Virtual Private Cloud which provides you a full control over your virtual netowrking environment. It enables us to connect our on-premises resources to AWS infrastructure through a virtual private network. For more informations: [https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)



**Following this need while setting up VPC**&#x20;

```
1. Create VPC -> Name, IP, 
2. Create Public subnet --> VPC_name, Name, IP
3. Create Private subnet --> VPC_name, Name, IP 
4. Create Internet Gateway -> Name
5. Create Route Table --> Name, Link_VPC, 
6. Go to route table--> edit_subnet_association, select both subnets
7. Create EC2 and link that network to new instances

Things to remember before we connect instances:
	1. Attached IGW to VPC
	2. Linked Instances subnet to Route Tables
	3. Add additional route on route table, Click on edit routes, add route set destination to 0.0.0.0/0 and set target to IGW


```

<figure><img src="../.gitbook/assets/Screen Shot 2022-11-06 at 4.35.32 PM.png" alt=""><figcaption></figcaption></figure>



<figure><img src="../.gitbook/assets/Screen Shot 2022-11-06 at 10.51.01 AM (1).png" alt=""><figcaption></figcaption></figure>
