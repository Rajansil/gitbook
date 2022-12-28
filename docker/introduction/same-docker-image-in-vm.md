# Same Docker image in VM

&#x20; **Running same image in AWS Virtal Machine**

Inorder to run the same image in VM we need to do following steps:\
\
**Step 1:** Create a repository in docker hub with name and push the image image to docker hub

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-27 at 10.09.00 PM.png" alt=""><figcaption></figcaption></figure>

**Step 2:** Once you create a repository, please login through terminal as well.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-27 at 10.11.23 PM.png" alt=""><figcaption></figcaption></figure>

**Step 3:** Now, need to tag our image to our private repository:\
&#x20;            **docker tag SOURCE\_IMAGE\[:TAG] TARGET\_IMAGE\[:TAG]**\
&#x20;   **Ex: docker tag test rajansilwal04/first\_test:tagname**

**Step 4:** Need to push our image to repository:\
&#x20;      **Ex: docker push rajansilwal04/first\_test:tagname**

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-27 at 10.20.05 PM.png" alt=""><figcaption></figcaption></figure>

**Step 5.** Create a EC2 instances ([https://app.gitbook.com/o/Sl85xsDKUHFlvowLtBga/s/9wnMXVEmEJ7k6VEhLJYN/\~/changes/aNCytkzQw3O9d4ixbcKU/cloud-aws/opennebula](../../cloud-aws/opennebula.md))

**Step 6:** Install Docker on Linux --> **yum install docker -y**

**Step 7:** Since our repository is public so we can pull that image directly from internet:\
&#x20;   Ex: **sudo docker** **run -d --rm --name test1 -p 80:80 test**
