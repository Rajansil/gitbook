# Setup Terraform in AWS

**Step 1:** Create a IAM user in AWS. Here is the process to create IAM user. \
[https://app.gitbook.com/o/Sl85xsDKUHFlvowLtBga/s/9wnMXVEmEJ7k6VEhLJYN/\~/changes/kPHMMpBOqOYvqWeP0spW/aws/opennebula-2](../../cloud-aws/opennebula-1.md)\
\
**Step 2:** Create one repository in Gitlab, clone that git repo in your local computer. Now, we need to install **AWS extension** on **VS code**. Once you add extension, go to **command palette** and search for **AWS:credentials profile.** Add your **aws\_access\_key\_id** and **aws\_secret\_access\_key.**\
\
**Step 3:** It will ask you to choose your zone. Please select one or more zone that you want to work on.&#x20;

**Step 4:** Open that repository that we create in our local computer.

**Step 5:** Create a file name called **providers.tf** inside our repo. Add following line of code in **providers.tf** which will link to our AWS console.&#x20;



```
# You can find this script in terrafrm page for AWS providers
terraform {
    required_providers {
        aws = {
            source  = "hashicorp/aws"
            version = "~> 4.0"
        }
    }
}
# It will link to our AWS account uisng access_key and secret_key
provider "aws" {
    region     = "us-west-1"
}
```

**Step 6:** Just to confirm we can run **terraform init** in terminal. Now it should show following output: Now, we have successfully initialized our terraform.\


<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-06 at 3.43.11 PM.png" alt=""><figcaption></figcaption></figure>
