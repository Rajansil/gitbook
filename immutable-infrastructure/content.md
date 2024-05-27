# 1️⃣ 1⃣ Content

**Overall Layout of of Immutable Infrastructure** \


* **Virt-Install(OS image)**
* **Base image(opennebula context)**
* **Golden Image (Basic company stuff)**
* **Ansible after-golden-image( All the remaining company packages )**
* **App Image( MYSQL, Kazoo, FreeSwitch and so on)**\


<figure><img src="../.gitbook/assets/Screen Shot 2022-10-22 at 1.37.14 PM.png" alt=""><figcaption></figcaption></figure>

In the **Current Pipeline** we were using a kickstart process where all the opennebula features were not use. If we need to create a multiple VM with same configuration then we used to do one at a time which was taking a lot of time. \
\
&#x20;**Suggested Pipeline** is using all the feature of opennebula feature as well as opennebula contexturization. This process take same number of time for multiple VM.
