# Create VPC using .tf

Here we will create a VPC using terraform. We will add following line of code in **main.tf** file



```
resource "aws_vpc" "usa_vpc"{
    cidr_block = "10.0.0.0/16"
    enable_dns_hostnames = true

    tags = {
        Name = "usa"
    }
}
```

**Run** \


```
terraform init
terraform plan # plan show overview of what will this code do
terraform apply 
```

Now we will see the following ouput after we **terraform apply**\


<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-06 at 4.15.12 PM.png" alt=""><figcaption></figcaption></figure>

Final step is to check in our AWS console page: Go to aws.console.amazon and go to VPC you will see the new VPC that we create from **terraform apply**\


<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-06 at 4.16.21 PM.png" alt=""><figcaption></figcaption></figure>
