# 🌥️ Load Balancer

It is a single point of contact for clients. The load balancer distributes incoming application traffic across multiple nodes, in multiple Availability Zones. Simply, It controls the incomming traffic into a different nodes. It helps for redundancy process.  For more information: \
[https://aws.amazon.com/elasticloadbalancing/](https://aws.amazon.com/elasticloadbalancing/)\


**Following this need while setting up VPC** \
\
**Step 1:** Create a VPC first -->Check out VPC module for more details on how to create VPC

```
1. Create Application Load Balancer -> Name, Scheme, IPv4, Network mapping( Select at leat 2 zone), Security Group,  
2. Create Target Group --> Select instances or IP , Name, select VPC, health check( set basic configurations)
	I. Setup Register Targets --> Select instances, Once we create a target group, now register the traffice to LB  
3. Set listern HTTP:80 to target group 
5. Finally, Our Load Balancer setup is ready

Under Basic Configuration of LB: You can see DNS(A record) to access.

```

<figure><img src="../.gitbook/assets/Screen Shot 2022-11-06 at 1.55.26 AM.png" alt=""><figcaption></figcaption></figure>

